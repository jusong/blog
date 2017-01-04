** svn常用命令集合


** 查看某个用户在某段时间的提交记录
svn log -r {2017-01-02}:{2017-01-05} -v | awk 'BEGIN{flag=0;} {if($0 ~ "jiafd"){flag=1;} else if($0 ~ "------------") {flag=0;} if(flag==1){print $0}}'
