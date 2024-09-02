### Настройка SSH keygen

git config --global [user.name](http://user.name/) "Onemyname"  
git config --global user.email  
konvvadim@yandex.ru  
ssh-keygen -o -t rsa -C "  
konvvadim@yandex.ru"

cat /home/vadim/.ssh/id_rsa.pub  
Копируешь SSH вставляешь в GitHub-> Setting>SSH and keygens -> new device