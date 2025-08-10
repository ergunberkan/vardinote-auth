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
              <p class="text-sm font-medium text-green-800">Email adresiniz başarıyla doğrulandı!</p>
            </div>
          </div>
        </div>

        <!-- Ana İçerik -->
        <div class="text-center">
          <h2 class="text-2xl font-semibold text-gray-900 mb-2">Email Doğrulama</h2>
          <p class="text-gray-500 text-sm mb-6">Hesabınızı aktifleştirmek için email doğrulama işlemi gerekiyor</p>
          
          <!-- Yükleniyor Animasyonu -->
          <div v-if="loading" class="flex justify-center items-center space-x-2">
            <div class="animate-spin rounded-full h-6 w-6 border-b-2 border-indigo-600"></div>
            <p class="text-sm text-gray-600">Doğrulama işlemi yapılıyor...</p>
          </div>
        </div>
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
const loading = ref(true)
const error = ref(null)
const success = ref(false)

onMounted(async () => {
  try {
    if (process.client) {
      const token = route.query.token
      const email = route.query.email

      console.log(' Gelen parametreler:', { token, email })
      console.log(' Full URL:', window.location.href)

      if (!token) {
        error.value = 'Doğrulama token\'ı bulunamadı.'
        return
      }

      // Önce token'ı exchange et
      console.log('🔄 Token exchange deneniyor...')
      const { data, error: exchangeError } = await client.auth.exchangeCodeForSession(token)
      
      if (exchangeError) {
        console.error('�� Exchange hatası:', exchangeError)
        
        // Exchange başarısızsa, verifyOtp dene
        console.log('🔄 VerifyOtp deneniyor...')
        const { data: verifyData, error: verifyError } = await client.auth.verifyOtp({
          email: email,
          token: token,
          type: 'signup'
        })
        
        if (verifyError) {
          console.error('🔴 VerifyOtp hatası:', verifyError)
          error.value = `Doğrulama hatası: ${verifyError.message}`
        } else {
          success.value = true
          console.log('✅ Email doğrulama başarılı (verifyOtp):', verifyData)
        }
      } else {
        success.value = true
        console.log('✅ Email doğrulama başarılı (exchange):', data)
      }

      // Başarılı doğrulama sonrası
      if (success.value) {
        // 3 saniye sonra Flutter uygulamasına yönlendir
        setTimeout(() => {
          window.location.href = 'vardinote://auth/confirm'
        }, 3000)
      }
    }
  } catch (err) {
    error.value = 'Beklenmeyen bir hata oluştu. Lütfen tekrar deneyin.'
    console.error('🔴 Beklenmeyen hata:', err)
  } finally {
    loading.value = false
  }
})
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