# Linux Basic Commands

```Shell
apt-get update
apt-get install sudo
sudo adduser platinum
usermod -aG sudo platinum
sudo apt install build-essential       (to download dev packages that includes libraries)

sudo apt update
sudo apt upgrade
Chmod 777 <filename>     (to give full privilege)
cd /mnt/c/Users/hp/OneDrive/Desktop/OMS\ Working/      (cd to local git repo from WLS)
cd c:/Users/hp/OneDrive/Desktop/OMS\ Working     (cd to local git repo from git bash)
```

## Rsync with progress bar
```Shell
rsync -azh --info=progress2 ~/Downloads/platinumecn.zip test_aws:/home/ubuntu/
rsync -azh --info=progress2 platinum@192.168.0.15:~/oms-drop-10/mfe-pats-web-content/dist.tar.gz ~/Desktop
rsync -azh --info=progress2 <source> <destination>
```

## Rsync from linux to linux
```Shell
rsync -azh oms-frontend platinum@192.168.0.20:/home/platinum/
rsync -azh <source> <ssh credentials>:<directory>
rsync -azh <source> <destination>
```

## Rsync from linux to windows and vice versa
```Shell
rsync -azh platinum@192.168.0.21:/home/platinum/oms-22042021/oms/27042021/ms-pats-web/ .
rsync -azh Users\hp\OneDrive\Desktop\jonathan . platinum@192.168.0.21:/home/platinum
```

## Access AWS instances
```shell
aws ec2 describe-instances --filter "Name=tag:Name,Values=Gitlab-Runner" --query 'Reservations[].Instances[].[InstanceId]'          #to get the instance id (can also get from ec2 instance)
aws ssm start-session --target i-08301447abe752fef
vim ~/.ssh    (add AWS ec2 instance information)
ssh postgres_aws     (ssh <name> in the .ssh file)
```

## Misc
```Shell
df -h                                      (to check hard disk space)
free -m                                    (to check ram usage)
sudo  chown -R platinum:platinum           (to change file user)
dos2unix <file>                            (change file format to linux base)
jps -l                                     (to check for any running java applications)
netstat -tuplean | grep <process id>       (to check specific java application tcp udp state "listening, waiting, etc")
netstat -tunlp                             (to check all java application tcp udp state "listening, waiting, etc")
curl --insecure -v https://www.google.com  (do it inside a container to check ssl certificate)
which sh   |   bash                        (to find the path of the shell / bash script)
pwd                                        (to find which path you are currently at)
ps -ef | grep java                         (to see the specific process name that is running currently)
kill <process id>                          (to kill a specific process)
find . -name "application.properties"      (to find a certain file name in this directory)
history | grep "java"                      (to find certain commands that you have typed in earlier)
tail -n 500 adaptor.logs > DBS-logs.txt    (to show the last 500 lines of the log file and put it in another txt file)
sudo rpm -ivh sample_file.rpm                (to install RPM file using rpm command)
```

## To check opened vi session file and kill the session
```Shell
ps -ef | grep vi
kill -9 17248
“17248 = session id”
```

## To run the .jar file
```Shell
java -server –jar <jar file>
java -server -jar ms-pats-order-management-api.jar
```

## To mount and unmount hard disk
```Shell
lsblk -f  (to check disk partitions)
mount /dev/sdc1 <location of where you want the drive to be>  (create one directory for it)
umount <folder path>
```