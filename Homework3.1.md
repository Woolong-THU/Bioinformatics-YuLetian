# bash
#!/bin/bash
if [ $# -ne 1 ]; then
    echo "错误：请提供一个目录作为参数。"
    echo "用法：$0 <目录>"
    exit 1
fi

target_dir="$1"

if [ ! -d "$target_dir" ]; then
    echo "错误：目录不存在：$target_dir"
    exit 1
fi

find "$target_dir" -mindepth 1 -maxdepth 1 -type f -print0 | while IFS= read -r -d '' file; do
    basename "$file"
done > filenames.txt

find "$target_dir" -mindepth 1 -maxdepth 1 -type d -print0 | while IFS= read -r -d '' dir; do
    basename "$dir"
done > dirname.txt

echo "处理完成！文件名保存在filenames.txt，子目录名保存在dirname.txt。"

# dirname
c1-RBPanno
app
bin
tmp_script
mljs
hub29
exRNA
download
node_modules
e-annotation
perl5
backup
rout
home
map2
script
script_backup
var
genome
postar_app
mogproject
ibme
git
RBP_map
highcharts
tcga
biosoft
postar.docker
test
l-lwl
module
postar2
tmp
a-docker
datatable
software
db
x-rbp

# filenames
chrom.size
a1.txt
name.txt
a.txt
test3.sh
human_geneExp.txt
insitiue.txt
.DS_Store
e1.txt
b.filter_random.pl
bam_wig.sh
b1.txt
number.sh
c1.txt
c.txt
image
d1.txt
test.txt
wigToBigWig
random.sh
if.sh
test.sh
out.bw
test4.sh
dir.txt
f1.txt
read.sh
mouse_geneExp.txt
