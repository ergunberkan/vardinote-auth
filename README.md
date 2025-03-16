# VardiNote Auth

VardiNote mobil uygulaması için kimlik doğrulama sayfaları. Bu proje, Supabase auth işlemleri için email doğrulama ve şifre sıfırlama sayfalarını içerir.

## Özellikler

- Email doğrulama
- Şifre sıfırlama
- Modern ve duyarlı tasarım
- Supabase entegrasyonu

## Teknolojiler

- Nuxt.js 3
- Tailwind CSS
- Supabase
- Vue 3

## Kurulum

1. Projeyi klonlayın:
```bash
git clone [repo-url]
```

2. Bağımlılıkları yükleyin:
```bash
npm install
```

3. Geliştirme sunucusunu başlatın:
```bash
npm run dev
```

## Yapılandırma

1. `.env` dosyası oluşturun ve Supabase bilgilerinizi ekleyin:
```
SUPABASE_URL=your-supabase-url
SUPABASE_KEY=your-supabase-anon-key
```

## URL'ler

- Site URL: https://vardinote-auth.vercel.app
- Email Doğrulama: https://vardinote-auth.vercel.app/confirm
- Şifre Sıfırlama: https://vardinote-auth.vercel.app/reset-password

## Supabase Yapılandırması

Supabase projenizde aşağıdaki URL'leri yapılandırın:

- Site URL: https://vardinote-auth.vercel.app
- Redirect URLs:
  - https://vardinote-auth.vercel.app/confirm
  - https://vardinote-auth.vercel.app/reset-password
