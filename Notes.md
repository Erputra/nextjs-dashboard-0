#   App Router
Dalam folder app/, ada beberapa file khsus dengan fungsi tertentu:

**File**                    **Fungsi**
layout.tsx                  Layout yang membungkus halaman dan mempertahankan state antar halaman.
page.tsx                    Entry point halaman berdasarkan routing folder.
loading.tsx                 Menampilkan UI loading saat halaman sedang dimuat.
error.tsx                   Menampilkan UI error jika terjadi error dalam komponen di bawahnya (child).
not-found.tsx               Menampilkan halaman 404 jika rute tidak di temukan.
head.tsx                    Menentukan <head> khusus untuk suatu halaman (tidak terlalu umum digunakan).
route.ts (khusus API)       Digunakan untuk membuat API route dalam app Router.
#

#   Cara membuat layouts dan pages.
Nextjs menggunakan file-system based routing, ini berarti kamu bisa menggunakan folder dan file untuk mendefinisikan rute.

Layout adalah UI yang akan di bagikan di semua page atau childrent di bawahnya, Layout akan mempertahankan state, tetap interactive dan tidak di render ulang. cara membuatnya adalah buat file layout.jsx kemudian dia harus dapat menerima children prop yang dapat berupa page atau layout lainnya.

```typescript
    export default function DashboardLayout({children,} : {
        childred: React.ReactNode
    }){
        return (
            <html lang="en">
                <body>
                    {/* Layout UI   */}
                    {/* children dimana kamu ingin merender sebuah halaman atau layout lainnya */}
                    <main>{children}</main>
                </body>
            </html>
        )
    }
```

layout diatas di sebut dengan `root layout` karena di definisikan pada root dari folder app. root layoyt membutuhkan dan harus mengandung html dan body tags.
#

#   Cara membuat Nested Route
Di Nextjs, kamu bisa menentukan route dengan membuat folder di dalam folder app/.
misalnya, kita ingin membuat halaman `/blog/[slug]`, maka kita buat struktur seperti ini.

app/
├── layout.tsx          <-- Layout utama
├── page.tsx            <-- Halaman utama (/)
├── blog/
│   ├── page.tsx        <-- Halaman /blog
│   ├── layout.tsx      <-- Layout khusus untuk /blog dan semua subhalaman di dalamnya
│   ├── [slug]/
│   │   ├── page.tsx    <-- Halaman /blog/[slug] (contoh: /blog/my-article)

Cara kerja Nextjs menentukan Halaman yang ditampilkan adalah:
1.  Saat membuka `/blog`, nextjs akan mencari `app/blog/page.tsx` untuk menampilkan halaman.
2.  Saat membuka `/blog/my-article`, nextjs akan mencari:
    🔹  `app/blog/[slug]/page.tsx`
    🔹  `[slug]` adalah folder dinamis yang menangkap nilai dari URL.
    🔹  Jika kamu membuka alamat `/blog/my-article`, maka nilai dari `slug = "my-article"`

📝 Contoh Implementasi Kode
1️⃣ `app/blog/page.tsx` → Halaman `/blog`
```typescript
    export default function BlogPage (){
        return {
            <main>
                <h1>Blog</h1>
                <p>Welcome to the blog page.</p>
            </main>
        }
    }
```
👉 Hasilnya: Saat kita akses /blog, akan muncul teks "Welcome to the blog page."

2️⃣ `app/blog/[slug]/page.tsx` → Halaman `/blog/[slug]`
```typescript
    export default function BlogPosts({params}: {params: { slug: string} }){
        return (
            <main>
                <h1>Blog Post: {params.slug}</h1>
                <p>This is the content of {params.slug}.</p>
            </main>
        )
    }
```
👉 Hasilnya: Jika kita mengunjungi `/blog/my-article`, halaman akan menampilkan:
Blog Post: my-article
This is the content of my-article.

🛠 Bagaimana Layout Bekerja di Nested Route?
Kita bisa menambahkan layout.tsx untuk memberikan layout khusus pada halaman /blog dan semua subhalamannya.

3️⃣ `app/blog/layout.tsx` → Layout Khusus untuk `/blog`
```typescript
    export default function BlogLayout({ children }: { children: React.ReactNode}){
        return (
            <section>
              <nav>
                <a href="/blog">Back to Blog</a>
              </nav>
              {children}
            </section>
        )
    }
```
✅ Hasilnya:
🔹  Semua halaman di dalam /blog akan memiliki navigasi "Back to Blog".
🔹  children berisi konten dari page.tsx yang sedang diakses.
#