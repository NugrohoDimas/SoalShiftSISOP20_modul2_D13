# Praktikum Modul 2 Sistem Operasi 2020


### Nomor 3
#### Soal :
Jaya adalah seorang programmer handal mahasiswa informatika. Suatu hari dia
memperoleh tugas yang banyak dan berbeda tetapi harus dikerjakan secara bersamaan
(multiprocessing).
- a. Program buatan jaya harus bisa membuat dua direktori di
“/home/[USER]/modul2/”. Direktori yang pertama diberi nama “indomie”, lalu
lima detik kemudian membuat direktori yang kedua bernama “sedaap”.
- b. Kemudian program tersebut harus meng-ekstrak file jpg.zip di direktori
“/home/[USER]/modul2/”. Setelah tugas sebelumnya selesai, ternyata tidak
hanya itu tugasnya.
- c. Diberilah tugas baru yaitu setelah di ekstrak, hasil dari ekstrakan tersebut (di
dalam direktori “home/[USER]/modul2/jpg/”) harus dipindahkan sesuai dengan
pengelompokan, semua file harus dipindahkan ke
“/home/[USER]/modul2/sedaap/” dan semua direktori harus dipindahkan ke
“/home/[USER]/modul2/indomie/”.
- d. Untuk setiap direktori yang dipindahkan ke “/home/[USER]/modul2/indomie/”
harus membuat dua file kosong. File yang pertama diberi nama “coba1.txt”, lalu
3 detik kemudian membuat file bernama “coba2.txt”.
(contoh : “/home/[USER]/modul2/indomie/{nama_folder}/coba1.txt”).</Br>
Karena Jaya terlalu banyak tugas dia jadi stress, jadi bantulah Jaya agar bisa membuat
program tersebut.
Catatan :
  - Tidak boleh memakai system().
  - Tidak boleh memakai function C mkdir() ataupun rename().
  - Gunakan exec dan fork
  - Direktori “.” dan “..” tidak termasuk
  
#### Penyelesaian :
##### Nomor 3a.<br/>
```c
if (cid == 0) {
char *argv[] = {"mkdir", "/home/dimas/Documents/Modul2/soal3/indomie", NULL};
execv("/bin/mkdir", argv);
}

if (cid2 == 0) {
sleep(5);
char *argv[] = {"mkdir", "/home/dimas/Documents/Modul2/soal3/sedaap", NULL};
execv("/bin/mkdir", argv);
}
```
Membuat folder dengan menggunakan execv 
##### Nomor 3b.<br/>
```c
if (cid3 == 0) {
char *argv[] = {"unzip", "/home/dimas/Documents/Modul2/soal3/jpg.zip", "-d", "/home/dimas/Documents/Modul2/soal3/", NULL};
execv("/usr/bin/unzip", argv);
}
``` 
Dengan menggunakan comment unzip untuk mengekstrak jpg.zip dari home/dimas/Documents/Modul2/soal3/jpg.zip ke /home/dimas/Documents/Modul2/soal3/
##### Nomor 3c.<br/>
```c
if (cid4 == 0) {

DIR *dp;
struct dirent *entry;
int file = 0;
char dir[50] = "/home/dimas/Documents/Modul2/soal3/jpg/" , dir2[50], dir3[50]="/home/dimas/Documents/Modul2/indomie/";

dp = opendir("/home/dimas/Documents/Modul2/soal3/jpg");
if(dp == NULL)
{
perror("Cannot open directory");
return(1);
}

while( (entry=readdir(dp))){
file++;
printf("File %3d: %d : %s\n",
file,
entry->d_type,entry->d_name
);
if (!strcmp (entry->d_name, "."))
continue;
if (!strcmp (entry->d_name, ".."))
continue;

strcpy(dir2,dir);
strcat(dir2,entry->d_name);
```
Digunakan untuk memfilter folder<br/>
Referensi [Herpur Tampan Menawan](https://stackoverflow.com/questions/8149569/scan-a-directory-to-find-files-in-c)

```c
if (cid8 == 0) {
DIR *dp;
struct dirent *entry;
int file = 0;
char dir[50] = "/home/dimas/Documents/Modul2/soal3/indomie/" , dir2[50];

dp = opendir("/home/dimas/Documents/Modul2/soal3/indomie");
if(dp == NULL){
perror("Cannot open directory");
return(1);
}

while( (entry=readdir(dp))){
file++;
printf("File %3d: %d : %s\n",
file,
entry->d_type,entry->d_name
);
if (!strcmp (entry->d_name, "."))
continue;
if (!strcmp (entry->d_name, ".."))
continue;
```
Digunakan untuk memfilter files

```c
if (cid5 == 0 && entry->d_type == 8) {
char *argv[] = {"mv",dir2,"/home/dimas/Documents/Modul2/soal3/sedaap/", NULL};
execv("/bin/mv", argv);
}

if (cid5 == 0 && entry->d_type == 4) {
char *argv[] = {"mv",dir2,"/home/dimas/Documents/Modul2/soal3/indomie/", NULL};
execv("/bin/mv", argv);
}
```
Perintah untuk memindahkan ke masing-masing direktori. Folder dipindah ke folder indomie sedangkan file-file dipindah ke folder sedaap

##### Nomor 3d.<br/>
```c
if (cid6 == 0) {
strcpy(dir2,dir);
strcat(dir2,entry->d_name);
strcat(dir2,"/coba1.txt");
printf("%s\n",dir2);
char *argv[] = {"touch",dir2, NULL};
execv("/usr/bin/touch", argv);
}

if (cid7 == 0) {
sleep(3);
strcpy(dir2,dir);
strcat(dir2,entry->d_name);
strcat(dir2,"/coba2.txt");
printf("%s\n",dir2);
char *argv[] = {"touch",dir2, NULL};
execv("/usr/bin/touch", argv);
}
```
Membuat coba1.txt dan coba2.txt. Untuk file coba2.txt akan dibuat setelah 3 detik dengan fungsi **sleep(3)**
