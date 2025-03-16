<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 flex flex-col justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-lg">
      <!-- Logo ve Başlık -->
      <div class="text-center mb-12">
        <h1 class="text-5xl font-bold bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent mb-4">
          Vardi<span class="text-indigo-600">Note</span>
        </h1>
        <p class="text-gray-500 text-lg">Kimlik Doğrulama Sayfası</p>
      </div>

      <div class="bg-white py-10 px-6 shadow-lg shadow-gray-200/50 sm:rounded-xl sm:px-12 border border-gray-100">
        <div class="space-y-8">
          <!-- Hoş Geldiniz Mesajı -->
          <div class="text-center">
            <div class="flex justify-center mb-6">
              <div class="rounded-full bg-indigo-100 p-3">
                <svg class="h-8 w-8 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"/>
                </svg>
              </div>
            </div>
            <h2 class="text-2xl font-semibold text-gray-900 mb-2">Hoş Geldiniz</h2>
            <p class="text-gray-500">Bu sayfa VardiNote uygulamasının kimlik doğrulama işlemleri için kullanılmaktadır.</p>
          </div>

          <!-- Özellikler -->
          <div class="grid grid-cols-1 gap-6 sm:grid-cols-2">
            <div class="bg-gray-50 p-4 rounded-lg border border-gray-100">
              <div class="flex items-center mb-2">
                <svg class="h-5 w-5 text-indigo-500 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/>
                </svg>
                <h3 class="font-medium text-gray-900">Email Doğrulama</h3>
              </div>
              <p class="text-sm text-gray-500">Hesabınızı aktifleştirmek için email doğrulama işlemi</p>
            </div>

            <div class="bg-gray-50 p-4 rounded-lg border border-gray-100">
              <div class="flex items-center mb-2">
                <svg class="h-5 w-5 text-indigo-500 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 7a2 2 0 012 2m4 0a6 6 0 01-7.743 5.743L11 17H9v2H7v2H4a1 1 0 01-1-1v-2.586a1 1 0 01.293-.707l5.964-5.964A6 6 0 1121 9z"/>
                </svg>
                <h3 class="font-medium text-gray-900">Şifre Sıfırlama</h3>
              </div>
              <p class="text-sm text-gray-500">Güvenli bir şekilde şifrenizi sıfırlama işlemi</p>
            </div>
          </div>

          <!-- Güvenlik Notu -->
          <div class="bg-indigo-50 rounded-lg p-4 border border-indigo-100">
            <div class="flex">
              <div class="flex-shrink-0">
                <svg class="h-5 w-5 text-indigo-400" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"/>
                </svg>
              </div>
              <div class="ml-3">
                <p class="text-sm text-indigo-700">
                  Bu sayfaya yalnızca VardiNote uygulaması üzerinden yönlendirilmelisiniz.
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Alt Bilgi -->
      <div class="mt-8 text-center">
        <p class="text-xs text-gray-500">
          &copy; {{ new Date().getFullYear() }} VardiNote. Tüm hakları saklıdır.
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
// Ana sayfa için ek bir işlem gerekmiyor
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