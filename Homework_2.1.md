# 行数字符数统计
```
wc -l test_command.gtf #行数
wc -m test_command.gtf #字符数
```
# grep命令
```
grep '^chr_' test_command.gtf | grep 'YDL248W'
```
# sed命令
```
sed 's/chr_/chromosome_/g' test_command.gtf | cut -f 1,3,4,5
```
# result.gtf
```
awk 'BEGIN{FS=OFS="\t"} {tmp=$2; $2=$3; $3=tmp; print}' test_command.gtf | sort -t$'\t' -k4,4n -k5,5n > result.gtf
```
# 权限
```
ls -hl
chmod 774 test_command.gtf
ls -hl
```
