# note5
&ensp;&ensp;&ensp;&ensp;centos7确定计划任务是否启动：systemctl  status  atd（at） crond(crond)
centos6确定计划任务是否启动：sercice  atd  status
                                                      service  crond  status
编辑计划任务的文件/etc/crontab
直接修改文件只适合root，普通用户无法直接修改文件
普通用户创建计划任务用命令：crontab  -e 
普通用户创建用的是vi    export  EDITOR=vim   修改变量
要想使配置文件对所有人生效，必须写到  /etc/profile.d/下（自己创建一个以.sh为后缀的文件）
所有创建的文件都计划任务文件都会放到/var/spool/cron/文件下
crontab -e -u chen管理员如果想修改普通用户的文件必须加-u 指定用户
文件/etc/log/cron 可以看计划任务的日志tail  -f /var/log/messages
判断成绩脚本
read -p "please  input your score: " score
if [[ "$score" =~ ^[0-9]+$ ]];then
        if [  "$score" -gt 100 ];then
                echo "your score is false "
        elif [ $score -lt 60 ];then
                echo "no pass"
        elif [ $score -lt 80 ];then
                echo "so so"
        else
                echo "very good"        
        fi
else
        echo "please input digit"
        exit
fi                                            

read -p "please  input your score: " score
if [[  ！ "$score" =~ ^[0-9]+$ ]];then  叹号表示取反
                echo "please input digit"
 elif [ "$score" -gt 100 ];then
                echo "your score is false "
elif [ $score -lt 60 ];then
                echo "no pass"
elif [ $score -lt 80 ];then
                echo "so so"
 else
                echo "very good"        
  fi                                      
  case语句：菜单
cat << EOF
1：饺子
2：面条
3：拉面
4：烩面
5:米线
EOF
read -p "please  choose  menu num: " menu
case  $menu in
1|2)
        echo "the price is 10 yuan"
        ;;
3)
        echo "the price is 15 yuan"
        ;;
4)
        echo "the price is 20 yuan"
        ;;
5)
        echo "the pric is 25 yuan "
        ;;
*)
        echo "input false"
esac                            
case语法条件判断，当第一个条件满足，执行第一个，依次类推。
当以上条件都不满足，执行默认分支
菜单脚本
read -p "Do you agree  (yes or no)? " chen
chen=`echo $chen | tr 'A-Z' 'a-z'`定义新的变量，把大写转化成小写。
命令里用反向单引号扩起来，
case  $chen in
y|yes)
        echo "yes"
        ;;
n|no)
        echo "no"
        ;;
*)
        echo "请按照正确格式"
        ;;
esac      
或者另一种写法
read -p "please  input  yes  or no ?"   chen
case  $chen in
[Yy]|[Yy][Ee][Ss])
          echo "yes"
          ;;
[Nn]|[Nn][Oo])
           echo "no”
           ；；
*)
        echo "qing chong xin shu ru "
  /etc/crontab  文件是计划任务的配置文件
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
30 2  * 3-6，12  *   表示3-6月和12月的每天2：30做备份   用户（root）  脚本路径
      白名单/etc/at.sllow  
黑名单/etc/at.deny(在黑名单里的用户，无法创建计划任务）
当一个用户去执行计划任务，系统会先看白名单，如果有，就可以执行。如果不在，直接拒绝， 
不看黑 名单（默认白名单是不存在的）   如果看黑名单，如果里面有，拒绝，没有将允许
当其中一次循环满足条件将执行continue，那么这一次循环将退出，执行下一次循环
i=1
until [ "$i" -gt 10 ];do                                                     
        let i++
        if [ "$i" -eq 5 ];then
                continue
        fi
        echo i=$i
done
命令let  i++要放到前面，否则循环将一直卡在if循环中

for((i=1;i<=10;i++));do
        if [ "$i" -eq 5 ];then
                break                                                        
        fi
        echo i=$i
done
命令break执行到满足if条件将退出整个循环

for((i=1;i<=10;i++));do
        for((j=1;j<=10;j++));do
                if [ "$j" -eq 5 ];then
                        continue
                fi
                echo j=$j
        done
done                            
for循环里套了for循环，for循环里有套了if判断，当里面的循环完，才会执行外面的整体循环

for((i=1;i<=3;i++));do
        for((j=1;j<=10;j++));do
                if [ "$j" -eq 5 ];then
                        continue 2
                fi
                echo j=$j
        done
done       
ift 顶掉第一个参数
echo 1st is $1sh
echo all args are $*
shift
echo shift
echo 1st is $1
echo all args are $*
shift
echo shift  
red(){
        echo -e "\033[41m         \033[0m\c"
}
yel(){
        echo -e "\033[43m         \033[0m\c"
}
redyel(){
        for((i=1;i<=4;i++));do
                for((j=1;j<=4;j++));do
                        [ "$1" = "-r" ] && { red;yel; }  || { yel;red; }
                done
                echo
        done
}
for((line=1;line<=8;line++));do
        [ $[$line%2 ]  -eq 0 ] && redyel || redyel -r
done                                        
finish(){
        echo "finish"                                                     
}
trap  finish exit
for((i=1;i<=10;i++));do
        let sum+=i
        sleep 1
done
echo $sum
可以在函数里定义要执行的命令，即使不是正常退出，需要的操作的也可以执行
