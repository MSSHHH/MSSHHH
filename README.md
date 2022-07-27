2022 7-27
在暑期数学建模培训中，我们做了2017ACT系统参数标定及成像这道题，在做第一题的过程中，我需要写程序读取Excel中的数据并将这些数据转化成一个两行n列的矩阵，并利用此矩阵将椭圆曲线方程拟合出来，
但是在这个过程中提取数据花费了我大量时间，我刚开始一直没有考虑到执行语句对行数的潜在影响，用两个for循环一直得不出结果，后来我把m=1放到了for循环外，并用m作为表示行数的变量，每次得到一个坐标后，都令
m+1，这样行数就不会被重复赋值，问题得到解决。



F=@(p,x)p(1)*x(:,1).^2+p(2)*x(:,1).*x(:,2)+p(3)*x(:,2).^2+p(4)*x(:,1)+p(5)*x(:,2)+p(6);
A=xlsread('1.2（降噪）.xlsx','A1:SR512');
Up=[]
m=1;
for i=190:359
    for j=1:512
        if A(j,i)==1
            Up(m,1)=i;
            Up(m,2)=j;
            m=m+1;
            continue;
        end
   end
end
UpX=Up(:,1);
UpY=Up(:,2);
% p0系数初值
p0=[1 1 1 1 1 1];
warning off
% 拟合系数，最小二乘方法
p =nlinfit(Up,zeros(size(Up,1),1),F,p0);
plot(UpX,UpY,'r.');
hold on;
 
UpMinx=min(UpX);
UpMaxx=max(UpX);
UpMiny=min(UpY);
UpMaxy=max(UpY);
 
% 作图
ezplot(@(x,y)F(p,[x,y]),[-1+UpMinx,1+UpMaxx,-1+UpMiny,1+UpMaxy]);
title('曲线拟合');
legend('样本点','拟合曲线')
