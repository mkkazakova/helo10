
```
$ sudo apt install vagrant #Установка
$ vagrant version #Версия
$ vagrant init bento/ubuntu-19.10 #инициализация виртуальной машины на ubuntu
$ less Vagrantfile #сгенерируется файл настроек виртуальной машины
Создаем новый Vagrantfile, перезаписываем изначальный (флаг -f), находящийся в текущем пути. 
Причем информация в нем будет в минимальном объеме (флаг -m)
$ vagrant init -f -m bento/ubuntu-19.10 
```

```
$ mkdir shared
```

```
$ cat > Vagrantfile <<EOF
\$script = <<-SCRIPT
sudo apt install docker.io -y
sudo docker pull fastide/ubuntu:19.04
sudo docker create -ti --name fastide fastide/ubuntu:19.04 bash
sudo docker cp fastide:/home/developer /home/
sudo useradd developer
sudo usermod -aG sudo developer
echo "developer:developer" | sudo chpasswd
sudo chown -R developer /home/developer
SCRIPT
EOF
```

Vagrantfile

```
Vagrant.configure("2") do |config|
  config.vagrant.plugins = ["vagrant-vbguest"]
  config.vm.box = "bento/ubuntu-19.10"
  config.vm.network "public_network"
  config.vm.synced_folder('shared', '/vagrant', type: 'rsync')
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "2048"
  end
  config.vm.provision "shell", inline: \$script, privileged: true
  config.ssh.extra_args = "-tt"
end
```

```
# проверка корректности файла Vagrantfile
$ vagrant validate ##валидация

$ vagrant status ##проверка статуса
$ vagrant up # --provider virtualbox ##запуск виртуальной машины
$ vagrant port ##просмотр порта
$ vagrant status
$ vagrant ssh ##подключение к виртуальной машине через ssh

$ vagrant snapshot list ##просмотр снимков виртуальных машин
$ vagrant snapshot push ##добавление снимка виртуальной машины
$ vagrant snapshot list
$ vagrant halt ##выключение виртуальной машины
$ vagrant snapshot pop ##открытие снимка виртуальной машины
```

```
$ vagrant plugin install vagrant-vmware-esxi ##установка плагина vmware
$ vagrant plugin list ##просмотр плагинов
$ vagrant up --provider=vmware_esxi ##запуск виртуальной машины с указанием провайдера
```


Использованные репы:
Коротко: https://github.com/dgt20u186/lab10-1
С комментами: https://github.com/dgt20u186/lab10-2
С тем, что выводит: https://github.com/dgt20u186/lab10-2
С комментами: https://gist.github.com/SCLOUDFER/e02e7505b00ada6258b4bc551fa08833
Содержимое файла: https://github.com/hashicorp/vagrant/blob/main/Vagrantfile
