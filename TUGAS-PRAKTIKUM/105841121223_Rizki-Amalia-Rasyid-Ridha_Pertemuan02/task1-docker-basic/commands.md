# Task 1: Eksplorasi Docker Dasar

Berikut adalah daftar perintah yang saya jalankan untuk menyelesaikan Task 1:

1. Menarik image Nginx dari Docker Hub:
   `docker pull nginx:alpine`

2. Menjalankan container dengan nama web-praktikum di background (port 8080):
   `docker run -d --name web-praktikum -p 8080:80 nginx:alpine`

3. Memeriksa container yang sedang berjalan:
   `docker ps`

4. Melihat log dari container:
   `docker logs web-praktikum`

5. Masuk ke dalam terminal container secara interaktif:
   `docker exec -it web-praktikum sh`

6. (Di dalam container) Melihat isi folder dan konfigurasi nginx:
   `ls -la /usr/share/nginx/html`
   `cat /etc/nginx/nginx.conf`
   `exit`

7. Menghentikan dan menghapus container:
   `docker stop web-praktikum`
   `docker rm web-praktikum`