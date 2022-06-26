# rfam
sudo apt update
sudo apt install apt-transport-https
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce
sudo systemctl status docker

sudo docker run -d --name jenkins_ci -p 8080:8080 jenkins:2.60.3
sudo docker ps 
sudo docker exec jenkins_ci cat /var/jenkins_home/secrets/initialAdminPassword

#подключиться к базе данных в командной строке:
mysql --user rfamro --host mysql-rfam-public.ebi.ac.uk --port 4497 --database Rfam

#Пайплайн должен запускать изменившийся скрипт и выводить результат запроса.
#простейший select, который будет просто выводить первую строку таблицы:
select * from fr.fram_acc limit 1;

# Она будет выбирать все РНК крыс, хранящиеся в этой базе данных:
SELECT fr.rfam_acc, fr.rfamseq_acc, fr.seq_start, fr.seq_end
FROM full_region fr, rfamseq rf, taxonomy tx
WHERE rf.ncbi_id = tx.ncbi_id
AND fr.rfamseq_acc = rf.rfamseq_acc
AND tx.ncbi_id = 10116 -- NCBI taxonomy id of Rattus norvegicus
AND is_significant = 1 -- exclude low-scoring matches from the same clan
