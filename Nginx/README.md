# Tugas Vagrant

![](https://blog.theodo.fr/wp-content/uploads/2017/07/Vagrant.png)

## Soal Sesilab Nginx

Buatlah sistem load balancing dengan 1 load balancer(nginx dan 2 worker(apache2), terapkan algoritma load balancing round-robin, least-connected, dan ip-hash.

Soal :

1. Buatlah Vagrantfile sekaligus provisioning-nya untuk menyelesaikan kasus.
2. Analisa apa perbedaan antara ketiga algoritma tersebut.
3. Biasanya pada saat membuat website, data user yang sedang login disimpan pada session. Sesision secara default tersimpan pada memory pada sebuah host. Bagaimana cara mengatasi masalah session ketika kita melakukan load balancing?

#### BACKUP


## A. Cara Main
### 1. Install Vagrant 3 biji
	- Set up 3 Vagrant. 1 Load balancer + Worker.
	

### 2. Provisioning Install software
	- Load Balancer : 
		# apt-get install nginx -y
		# apt-get install php7.0-fpm -y

	- Worker :
		# apt-get install apache2
		# apt-get install libapache2-mod-php7.0 php7.0-fpm -y


### 3. Setup Worker ama Load Balancer nya
	- Worker : 
		# Set up worker nya biar bisa baca file PHP. Edit file.conf berikut di kedua file .conf worker :

		SS PASTE Disini, fileconfig isi disini

	- Load Balancer :
		# Set up LB nya di file default di /etc/nginx/sites-available/ (yang sudah di SYMLINK) dengan tambahan kode berikut :

		ISI DISINI FILE CONFIGNYA


### C. Membuat Virtualisasi
Setelah berhasil menginstall vagrant, selanjutnya kita akan mencoba membuat virtualisasi baru menggunakan provider virtualbox. Langkah-langkah pembuatan virtualisasi baru dijelaskan sebagai berikut:

1. Buat folder baru untuk meletakkan konfigurasi.

	`mkdir vagrant-example`
2. Masuk ke dalam folder yang telah di buat.

	`cd vagrant-example`
3. Inisialisasi projek vagrant

	`vagrant init`

	Setelah menjalankan perintah diatas akan dibuat file baru bernama **Vagrantfile**
5. Isi dari **Vagrantfile**
	
		# -*- mode: ruby -*-
		# vi: set ft=ruby :

		# All Vagrant configuration is done below. The "2" in Vagrant.configure
		# configures the configuration version (we support older styles for
		# backwards compatibility). Please don't change it unless you know what
		# you're doing.
		Vagrant.configure("2") do |config|
		  # The most common configuration options are documented and commented below.
		  # For a complete reference, please see the online documentation at
		  # https://docs.vagrantup.com.

		  # Every Vagrant development environment requires a box. You can search for
		  # boxes at https://vagrantcloud.com/search.
		  config.vm.box = "base"

		  # Disable automatic box update checking. If you disable this, then
		  # boxes will only be checked for updates when the user runs
		  # `vagrant box outdated`. This is not recommended.
		  # config.vm.box_check_update = false

		  # Create a forwarded port mapping which allows access to a specific port
		  # within the machine from a port on the host machine. In the example below,
		  # accessing "localhost:8080" will access port 80 on the guest machine.
		  # NOTE: This will enable public access to the opened port
		  # config.vm.network "forwarded_port", guest: 80, host: 8080

		  # Create a forwarded port mapping which allows access to a specific port
		  # within the machine from a port on the host machine and only allow access
		  # via 127.0.0.1 to disable public access
		  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

		  # Create a private network, which allows host-only access to the machine
		  # using a specific IP.
		  # config.vm.network "private_network", ip: "192.168.33.10"

		  # Create a public network, which generally matched to bridged network.
		  # Bridged networks make the machine appear as another physical device on
		  # your network.
		  # config.vm.network "public_network"

		  # Share an additional folder to the guest VM. The first argument is
		  # the path on the host to the actual folder. The second argument is
		  # the path on the guest to mount the folder. And the optional third
		  # argument is a set of non-required options.
		  # config.vm.synced_folder "../data", "/vagrant_data"

		  # Provider-specific configuration so you can fine-tune various
		  # backing providers for Vagrant. These expose provider-specific options.
		  # Example for VirtualBox:
		  #
		  # config.vm.provider "virtualbox" do |vb|
		  #   # Display the VirtualBox GUI when booting the machine
		  #   vb.gui = true
		  #
		  #   # Customize the amount of memory on the VM:
		  #   vb.memory = "1024"
		  # end
		  #
		  # View the documentation for the provider you are using for more
		  # information on available options.

		  # Enable provisioning with a shell script. Additional provisioners such as
		  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
		  # documentation for more information about their specific syntax and use.
		  # config.vm.provision "shell", inline: <<-SHELL
		  #   apt-get update
		  #   apt-get install -y apache2
		  # SHELL
		end

4. Pada contoh kasus ini kita ingin menggunakan os Ubuntu (12.04) Precise 64 bit. Maka dari itu kita perlu download **Vagrant Box** terlebih dahulu. Dengan cara:

	`vagrant box add hashicorp/precise64`

5. Kemudian jika box mendukung lebih dari satu provider akan ditanyakan provider yang akan digunakan. Pilih provider **virtualbox**.
6. Setting box pada Vagrant file dengan cara ganti vm.box yang awalnya **base** menjadi **hashicorp/precise64**

		config.vm.box = "base"
	menjadi

		config.vm.box = "hashicorp/precise64"
7. Simpan file Vagrantfile

### D. Konfigurasi Resource Virtual Machine
Layaknya komputer fisik, virtual machine terdapat memory dan core cpu. Pada **Vagrant**, resource core dan memory virtual machine diatur pada **Vagrantfile**. Untuk mengatur resource virtual machine dapat dilakukan dengan langkah berikut:

1. Uncomment konfigurasi

		config.vm.provider "virtualbox" do |vb|
		   # Display the VirtualBox GUI when booting the machine
		   # vb.gui = true
		
		   # Customize the amount of memory on the VM:
			vb.memory = "1024"
		end
2. Untuk menentukan resource memory yang diperlukan ganti value 

	`vb.memory`
3. Untuk mengatur core cpu, tambahkan baris sebelum **end**.

		config.vm.provider "virtualbox" do |vb|
		   # Display the VirtualBox GUI when booting the machine
		   # vb.gui = true
		
		   # Customize the amount of memory on the VM:
			vb.memory = "1024"
			vb.cpus = 2 
		end

4. Untuk storage tidak dapat diatur oleh vagrant. Mungkin bisa diatur melalui Hypervisor yang digunakan.

### E. Cara Bermain
1. Menjalankan vagrant : `vagrant up`
2. Untuk masuk ke virtual machine : `vagrant ssh`
3. Untuk mematikan virtual machine : `vagrant halt`
4. Untuk menghapus virtual machine : `vagrant destroy`
5. Untuk merestart virtual machine dan akan memuat ulang konfigurasi **Vagrantfile** : `vagrant reload`
6. Untuk menjalankan provisioning : `vagrant provision`

### F. Konfigurasi Internet Pada Virtual Machine
Pada vagrant terdapat 3 jenis konfigurasi agar virtual machine dapat diakses dari host maupun dari luar host.

1. Port Forwarders

	Pada linkungan produksi, aplikasi yang berjalan di virtualisasi harus dapat diakses melalui komputer hostnya. Akses ini dilakukan dengan mekanisme port forwarders . Port forwarders bekerja dengan cara meneruskan akses dari port komputer host menuju port tertentu pada virtualisasi. Port forwarders dapat diilustrasikan dengan gambar berikut. 

	![](assets/port_forwarder.png)


	Klien mengakses port 8080/8443 pada komputer host, kemudian akan diteruskan menuju port 80/443 pada virtualisasi vagrant. Untuk mengaktifkan port forwarders, terdapat step-step sebagai berikut:

	1. Uncomment baris agar port 80 VM dapat diakses melalui port 8080 host

			config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
		dan hapus 

			host_ip: "127.0.0.1"
		agar bisa diakses dari luar host. Sehingga menjadi

			config.vm.network "forwarded_port", guest: 80, host: 8080
	2. Tambahkan baris

			config.vm.network "forwarded_port", guest: 443, host: 8443
		agar port 443 pada virtual machine dapat diakses melalui port 8443
2. Static Local IP

	Ketika banyak virtualisasi yang dijalankan, kita membutuhkan alamat IP lokal agar antar virtualisasi dapat saling berkomunikasi. IP lokal hanya dapat diakses oleh komputer host dan virtualisasi pada komputer host yang sama. Untuk menentukan alamat IP pada virtualisasi lakukan step-step berikut:

	1. Uncomment baris :

			config.vm.network "private_network", ip: "192.168.33.10"
		Virtual machine dapat kita akses melalui ip **192.168.33.10**

		Untuk pemilihan IP, pilihlah ip dengan angka belakang 2-254, karena ip dengan angka belakang 1 sudah digunakan oleh komputer host sebagai router antar virtualisasi. Alamat IP lokal antar virtualisasi tidak perlu harus dalam subnet yang sama. Alamat IP pada subnet yang berbeda tetap bisa berkomunikasi, karena mekanisme routing telah diatur oleh vagrant.
3. Bridged IP

	Pada kondisi tertentu dibutuhkan virtualisasi yang dapat di akses dari luar, contohnya pada layanan cloud vps(virtual private server). Untuk membuat virtualisasi dapat diakses dari luar, dibutuhkan mekanisme yang disebut bridging . Mekanisme bridging ini telah ditangani oleh vagrant. Untuk mengaktifkan fungsi bridged ikuti langkah-langkah berikut:

	* Untuk DHCP: 
		1. Uncomment baris:

				config.vm.network "public_network"

	* Untuk konfigurasi IP static pada virtualiasasi gunakan konfigurasi berikut

			config.vm.network "public_network", ip: "10.151.36.225"

		Ip tergantung subnet dari host

### G. Sinkronisasi folder
Adakalanya kita ingin mendevelop aplikasi menggunakan editor favorit kita seperti sublimetext, netbeans dan lain sebagainya, tetapi kita ingin agar kode-kode aplikasi kita tersebut berada di dalam virtualisasi sehingga ketika terjadi perubahan kode maka kode tersebut langsung dipindahkan ke dalam virtualisasi atau kita ingin folder di dalam virtualisasi dapat diakses melalui komputer host untuk keperluan backup/audit. Hal seperti ini dapat dilakukan menggunakan fitur sinkronisasi pada vagrant. Folder yang di sinkronisasi dapat diakses melalui komputer host atau virtual dengan kondisi tersinkronisasi, sehingga ketika terjadi perubahan melalui komputer host atau melalui virtualisasi data-data dalam folder tersebut tetap sama. Untuk mengaktifkan sinkronisasi folder lakukan langkah-langkah berikut:

1. Buka file Vagrantfile ubah baris berikut.

		# config.vm.synced_folder "../data", "/vagrant_data"
	menjadi

		config.vm.synced_folder "src/", "/var/www"
2. Simpan file Vagrantfile. **src/** adalah folder pada komputer host, sedangkan **/var/www** adalah folder pada virtual machine.

3. Buat folder src di dalam folder projek vagrant example kemudian tambahkan file index.html

		mkdir src
		echo "hello world" >> src/index.html
4. Jalankan virtualisasi.

		vagrant up

5. Masuk ke dalam virtualisasi.

		vagrant ssh

6. Lakukan perubahan pada file src/index.html di komputer host, kemudian cek file index.html yang berada pada folder /var/www di virtual machine. Kedua file akan berisi data yang sama, karena telah tersinkronisasi.

### H.Provisioning aplikasi pada virtual machine
Kita menginginkan komputer virtual yang kita gunakan telah terinstall aplikasi aplikasi yang kita butuhkan. Tahapan instalasi dan konfigurasi tersebut sering dikenal dengan sebutan provisioning. Pada vagrant, provisioning dapat dilakukan dengan mudah. Kita dapat membuat script menggunakan bash scripting untuk melakukan provisioning. Langkah-langkah untuk melakukan provisioning adalah sebagai berikut:

* Menggunakan File bootstrap

	1. Buat bash script dengan nama bootsrap.sh pada folder yang sama dengan vagrant file.
	2. Untuk menginstall apache tuliskan baris berikut pada file bootsrap.sh.

			#!/usr/bin/env bash
			apt-get update
			apt-get install -y apache2
	3. Pada file Vagrantfile diatas **end** terakhir, tambahkan baris  

			config.vm.provision :shell, path: "bootstrap.sh".

		Sehingga menjadi
		
				config.vm.provision "shell", path: "bootstrap.sh"
			end

	4. Simpan file Vagrantfile kemudian nyalakan virtualisasi.

			vagrant up

	5. Jika virtualisasi sudah dibuat dan sedang menyala maka jalankan fungsi reload dengan menambahkan flag **--provision** untuk memaksa vagrant merestart virtualisasi dan menjalankan script provisioning ketika mesin virtual sedang aktif.

			vagrant reload --provision

		atau tanpa merestart ulang virtual machine

			vagrant provision

	6. Cek apakah provisioning berhasil dengan masuk kedalam virtualisasi menggunakan ssh.

			vagrant ssh
	7. Cek apakah apache telah berhasil terinstall

			service apache2 status
* atau dengan Menambahkan command pada **Vagrantfile**
	1. Uncomment baris:

			config.vm.provision "shell", inline: <<-SHELL
			  	apt-get update
			  	apt-get install -y apache2
			SHELL


Proses provisioning dapat juga menggunakan configuration management seperti ansible, chef, atau puppet. Proses provisioning otomatis menjalankan menggunakan superuser. Jika ingin mematikan superuser dapat menambahkan opsi

	privileged:false

Contoh:

	config.vm.provision "shell", path: "bootstrap.sh",  privileged:false