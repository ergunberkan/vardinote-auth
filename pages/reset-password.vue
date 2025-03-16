<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 flex flex-col justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-md">
      <!-- Logo ve Başlık -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent">
          Vardi<span class="text-indigo-600">Note</span>
        </h1>
      </div>

      <div class="bg-white py-8 px-4 shadow-lg shadow-gray-200/50 sm:rounded-xl sm:px-10 border border-gray-100">
        <!-- Hata Mesajı -->
        <div v-if="error" 
          class="rounded-lg bg-red-50 p-4 mb-6 border border-red-100 animate-fade-in">
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <svg class="h-5 w-5 text-red-400" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd"/>
              </svg>
            </div>
            <div class="ml-3">
              <p class="text-sm font-medium text-red-800">{{ error }}</p>
            </div>
          </div>
        </div>

        <!-- Başarı Mesajı -->
        <div v-if="success" 
          class="rounded-lg bg-green-50 p-4 mb-6 border border-green-100 animate-fade-in">
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <svg class="h-5 w-5 text-green-400" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"/>
              </svg>
            </div>
            <div class="ml-3">
              <p class="text-sm font-medium text-green-800">Şifreniz başarıyla güncellendi!</p>
            </div>
          </div>
        </div>

        <!-- Ana İçerik -->
        <div class="text-center mb-6">
          <h2 class="text-2xl font-semibold text-gray-900 mb-2">Şifre Sıfırlama</h2>
          <p class="text-gray-500 text-sm">Lütfen yeni şifrenizi belirleyin</p>
        </div>

        <form @submit.prevent="handleSubmit" class="space-y-6">
          <div>
            <label for="password" class="block text-sm font-medium text-gray-700">Yeni Şifre</label>
            <div class="mt-1">
              <input
                id="password"
                v-model="password"
                type="password"
                required
                class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-lg shadow-sm placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                placeholder="En az 6 karakter"
              />
            </div>
          </div>

          <div>
            <label for="confirmPassword" class="block text-sm font-medium text-gray-700">Şifre Tekrar</label>
            <div class="mt-1">
              <input
                id="confirmPassword"
                v-model="confirmPassword"
                type="password"
                required
                class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-lg shadow-sm placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                placeholder="Şifrenizi tekrar girin"
              />
            </div>
          </div>

          <div>
            <button
              type="submit"
              :disabled="loading"
              class="w-full flex justify-center py-2.5 px-4 border border-transparent rounded-lg shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 transition-colors duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
            >
              <svg v-if="loading" class="animate-spin -ml-1 mr-2 h-4 w-4 text-white" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              {{ loading ? 'İşlem yapılıyor...' : 'Şifreyi Güncelle' }}
            </button>
          </div>
        </form>
      </div>

      <!-- Alt Bilgi -->
      <div class="mt-6 text-center">
        <p class="text-xs text-gray-500">
          &copy; {{ new Date().getFullYear() }} VardiNote. Tüm hakları saklıdır.
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
const client = useSupabaseClient()
const route = useRoute()
const loading = ref(false)
const error = ref(null)
const success = ref(false)
const password = ref('')
const confirmPassword = ref('')

const handleSubmit = async () => {
  if (password.value !== confirmPassword.value) {
    error.value = 'Şifreler eşleşmiyor.'
    return
  }

  if (password.value.length < 6) {
    error.value = 'Şifre en az 6 karakter olmalıdır.'
    return
  }

  loading.value = true
  error.value = null

  try {
    const { error: err } = await client.auth.updateUser({
      password: password.value
    })

    if (err) {
      error.value = 'Şifre güncelleme işlemi başarısız oldu. Lütfen tekrar deneyin.'
      console.error('Güncelleme hatası:', err)
    } else {
      success.value = true
      password.value = ''
      confirmPassword.value = ''
    }
  } catch (err) {
    error.value = 'Bir hata oluştu. Lütfen tekrar deneyin.'
    console.error('Beklenmeyen hata:', err)
  } finally {
    loading.value = false
  }
}
</script>

<style>
@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-in {
  animation: fade-in 0.3s ease-out;
}
</style> 