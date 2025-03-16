<template>
  <div class="min-h-screen bg-gray-100 flex flex-col justify-center py-12 sm:px-6 lg:px-8">
    <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
      <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
        <div v-if="error" class="rounded-md bg-red-50 p-4 mb-4">
          <div class="flex">
            <div class="ml-3">
              <h3 class="text-sm font-medium text-red-800">Hata</h3>
              <div class="mt-2 text-sm text-red-700">
                <p>{{ error }}</p>
              </div>
            </div>
          </div>
        </div>
        
        <div v-if="success" class="rounded-md bg-green-50 p-4 mb-4">
          <div class="flex">
            <div class="ml-3">
              <h3 class="text-sm font-medium text-green-800">Başarılı</h3>
              <div class="mt-2 text-sm text-green-700">
                <p>Email adresiniz başarıyla doğrulandı!</p>
              </div>
            </div>
          </div>
        </div>

        <div class="text-center">
          <h2 class="text-2xl font-bold text-gray-900 mb-4">Email Doğrulama</h2>
          <p v-if="loading" class="text-gray-600">Doğrulama işlemi yapılıyor...</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const client = useSupabaseClient()
const route = useRoute()
const loading = ref(true)
const error = ref(null)
const success = ref(false)

onMounted(async () => {
  try {
    // URL'den code parametresini al
    const code = route.query.code

    if (!code) {
      error.value = 'Geçersiz doğrulama bağlantısı.'
      return
    }

    const { error: err } = await client.auth.verifyOtp({
      token: code,
      type: 'signup'
    })
    
    if (err) {
      error.value = 'Email doğrulama işlemi başarısız oldu. Lütfen tekrar deneyin.'
      console.error('Doğrulama hatası:', err)
    } else {
      success.value = true
    }
  } catch (err) {
    error.value = 'Bir hata oluştu. Lütfen tekrar deneyin.'
    console.error('Beklenmeyen hata:', err)
  } finally {
    loading.value = false
  }
})
</script> 