# DrawLinesToBlock

# AutoLISP DrawLinesToBlock

## Deskripsi

`DrawLinesToBlock` adalah skrip AutoLISP untuk AutoCAD yang menggambar garis dari objek teks (TEXT, MTEXT) ke objek blok. Fungsi ini memungkinkan pengguna untuk memilih apakah panjang garis yang digambar akan ditampilkan di tengah garis dalam format angka bulat.

## Fitur

- **Menghubungkan Teks ke Blok**: Menggambar garis dari objek teks yang dipilih menuju objek blok.
- **Opsi Panjang Garis**: Menampilkan panjang garis sebagai angka bulat di tengah garis jika diinginkan.

## Kode

Berikut adalah kode lengkap dari fungsi AutoLISP `DrawLinesToBlock`:
```markdown
```lisp
(defun show-usage-instructions ()
  (alert (strcat
    "Cara menggunakan fungsi DrawLinesToBlock:\n\n"
    "1. Ketik 'DrawLinesToBlock' di command line AutoCAD.\n"
    "2. Pilih objek teks yang ingin dihubungkan ke blok.\n"
    "3. Pilih objek blok yang menjadi tujuan garis.\n"
    "4. Saat diminta, pilih apakah Anda ingin menampilkan panjang garis (Y/N).\n\n"
    "Catatan: Fungsi ini bekerja dengan objek TEXT, MTEXT, dan BLOCK.\n\n"
    "Dibuat oleh: Syaiful Wachid\n"
    "Senior Project Designer: Fiberhome Indonesia\n"
    "Linkedin Profile: https://www.linkedin.com/in/syaiful-wachid-5373n/"
  ))
)

(defun c:copylinkedinprofilelink ()
  (setq linkedin-url "https://www.linkedin.com/in/syaiful-wachid-5373n/")
  (alert (strcat "Salin link profil LinkedIn berikut:\n\n" linkedin-url))
  (princ (strcat "\nLink profil LinkedIn: " linkedin-url))
  (princ)
)

(defun c:DrawLinesToBlock (/ textList blockObj blockPt textObj textPt lineObj lineLen midPt displayLength)
  ;; Memilih semua objek teks
  (setq textList (ssget '((0 . "TEXT,MTEXT"))))

  ;; Memilih objek blok
  (princ "\nPilih objek blok: ")
  (setq blockObj (car (entsel)))

  ;; Mendapatkan titik penyisipan dari blok yang dipilih
  (setq blockPt (cdr (assoc 10 (entget blockObj))))

  ;; Tanya apakah user ingin menampilkan panjang garis
  (setq displayLength (getstring "\nTampilkan panjang garis? (Y/N): "))

  ;; Iterasi melalui semua objek teks yang dipilih
  (if textList
    (progn
      (repeat (setq i (sslength textList))
        ;; Dapatkan objek teks
        (setq textObj (ssname textList (setq i (- i 1))))
        ;; Dapatkan titik dasar dari teks
        (setq textPt (cdr (assoc 10 (entget textObj))))
        ;; Gambar garis dari teks ke blok
        (setq lineObj (entmake
          (list
            (cons 0 "LINE")
            (cons 10 textPt)
            (cons 11 blockPt)
          )
        ))
        ;; Hitung panjang garis
        (setq lineLen (distance textPt blockPt))
        ;; Dapatkan titik tengah garis
        (setq midPt (list (/ (+ (car textPt) (car blockPt)) 2)
                          (/ (+ (cadr textPt) (cadr blockPt)) 2)
                    ))
        ;; Jika user memilih untuk menampilkan panjang garis
        (if (or (= displayLength "Y") (= displayLength "y"))
          (entmake
            (list
              (cons 0 "TEXT")
              (cons 10 midPt)
              (cons 40 0.2) ;; Ukuran teks
              (cons 1 (rtos (fix lineLen) 2 0)) ;; Panjang garis sebagai angka bulat
              (cons 7 "Standard") ;; Style teks
            )
          )
        )
      )
    )
    (princ "\nTidak ada objek teks yang dipilih.")
  )

  (princ)
)

;; Menjalankan instruksi penggunaan saat kode di-load
(vl-load-com)
(show-usage-instructions)
(princ "\nKetik DrawLinesToBlock untuk menjalankan fungsi menggambar garis.")
(princ "\nKetik copylinkedinprofilelink untuk menyalin link profil LinkedIn.")
(princ)

;; Cara Compile ke FAS
;(vlisp-compile 'st "path/to/DrawLinesToBlock.lsp")
```

## Cara Menggunakan

1. **Muat File LISP**:
   - Muat file LISP ke dalam AutoCAD dengan menggunakan perintah `APPLOAD`.

2. **Jalankan Fungsi**:
   - Ketik `DrawLinesToBlock` di command line AutoCAD dan tekan Enter.

3. **Pilih Objek Teks**:
   - Pilih satu atau beberapa objek teks yang ingin dihubungkan ke blok.

4. **Pilih Objek Blok**:
   - Pilih objek blok yang menjadi tujuan garis.

5. **Tampilkan Panjang Garis**:
   - Ketika diminta, ketik `Y` (Ya) jika Anda ingin menampilkan panjang garis di tengah garis yang digambar, atau `N` (Tidak) jika tidak ingin menampilkan panjang garis.

## Informasi Kontak

- **Dibuat oleh**: Syaiful Wachid
- **Senior Project Designer**: Fiberhome Indonesia
- **LinkedIn Profile**: [https://www.linkedin.com/in/syaiful-wachid-5373n/](https://www.linkedin.com/in/syaiful-wachid-5373n/)

## Lisensi

[Include any licensing information here, if applicable]

## Kompilasi ke FAS

Jika Anda ingin meng-compile file LISP ini menjadi file FAS, gunakan perintah berikut di AutoCAD Lisp Editor:

```lisp
(vlisp-compile 'st "path/to/DrawLinesToBlock.lsp")
```

Gantilah `path/to/DrawLinesToBlock.lsp` dengan path lengkap ke file LISP Anda.

```

Anda dapat menyesuaikan bagian lisensi jika diperlukan dan menambahkan informasi tambahan sesuai dengan kebutuhan proyek Anda.
