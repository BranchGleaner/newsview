<script>
    locker("QiShengZhaoHuan");
</script>

# 12.7作业

## 免责声明及警告

以下代码仅作参考，不保证准确性、鲁棒性或最优性，也没有任何人应当或可能为此负责。公布代码仅用于相互交流、讨论或学习，不得用于任何违法、违规或违纪之用途；违者自行承担全部责任和损失，自行为自己的前途负责。

代码尽力为所有题目提供容易被理解的解答。若有疏漏之处或其它意见，欢迎讨论。

## 一

### 代码设计

> 这次可是学聪明了，再也不想写的那么麻烦了(ノへ￣、)

```c
#include <stdio.h>
#include <math.h>

const int dic[]={0,31,28,31,30,31,30,31,31,30,31,30,31};

struct date
{
	int month;
	int day;
	int year;
};

int sign(int n)
{
	return (n!=0)?(n/abs(n)):0;
}

int isspe(int y)
{
	if((y%4==0&&y%100!=0)||y%400==0)
	{
		return 1;
	}
	return 0;
}

int fullday(int m,int f)
{
	return (m==2&&f==1)?dic[m]+1:dic[m];
}

int day_of_year(struct date d)
{
	int ans=0;
	for(int i=1;i<=d.month;i++)
	{
		if(i==d.month)
		{
			ans+=d.day;
		}
		else
		{
			ans+=fullday(i,isspe(d.year));
		}
	}
	return ans;
}

int compare_dates(struct date d1, struct date d2)
{
	if(d1.year!=d2.year)
	{
		return sign(d1.year-d2.year);
	}
	else
	{
		return sign(day_of_year(d1)-day_of_year(d2));
	}
}

int main()
{
	struct date d1,d2;
	int ans=0;
	printf("Please enter the year, month, and day of the first date, separated by a space between each data:\n");
	scanf("%d %d %d",&d1.year,&d1.month,&d1.day);
	printf("It is the %d day of that year.\n",day_of_year(d1));
	printf("Please enter the year, month, and day of the second date, separated by a space between each data:\n");
	scanf("%d %d %d",&d2.year,&d2.month,&d2.day);
	printf("It is the %d day of that year.\n",day_of_year(d2));
	ans=compare_dates(d1,d2);
	printf("The value returned by the comparison function is %d.\n",ans);
	if(ans==0)
	{
		printf("This indicates that the first and second dates are the same day.");
	}
	else if(ans==-1)
	{
		printf("This indicates that the first date is before the second.");
	}
	else
	{
		printf("This indicates that the first date is after the second.");
	}
	return 0;
}
```

### 运行展示

> **应该不会有人抄运行展示吧，不会吧不会吧(* ￣︿￣)**

#### 示例Ⅰ

```
Please enter the year, month, and day of the first date, separated by a space between each data:
2022 12 8
It is the 342 day of that year.
Please enter the year, month, and day of the second date, separated by a space between each data:
2020 12 8
It is the 343 day of that year.
The value returned by the comparison function is 1.
This indicates that the first date is after the second.
```

#### 示例Ⅱ

```
Please enter the year, month, and day of the first date, separated by a space between each data:
2022 2 23
It is the 54 day of that year.
Please enter the year, month, and day of the second date, separated by a space between each data:
2022 5 4
It is the 124 day of that year.
The value returned by the comparison function is -1.
This indicates that the first date is before the second.
```

#### 示例Ⅲ

```
Please enter the year, month, and day of the first date, separated by a space between each data:
2000 3 10
It is the 70 day of that year.
Please enter the year, month, and day of the second date, separated by a space between each data:
2000 3 10
It is the 70 day of that year.
The value returned by the comparison function is 0.
This indicates that the first and second dates are the same day.
```

## 二

### 代码设计

```c
#include <stdio.h>

struct time
{
	int hours;
	int minutes;
	int seconds;
};

struct time split_time(long total_seconds)
{
	struct time contime;
	contime.hours=total_seconds/3600;
	contime.minutes=(total_seconds%3600)/60;
	contime.seconds=total_seconds-contime.hours*3600-contime.minutes*60;
	return contime;
}

int main()
{
	long total;
	printf("Please enter the number of seconds from midnight:\n");
	scanf("%ld",&total);
	struct time times=split_time(total);
	printf("The equivalent time is %d hours, %d minutes, and %d seconds.",times.hours,times.minutes,times.seconds);
	return 0;
}
```

### 运行展示

#### 示例Ⅰ

```
Please enter the number of seconds from midnight:
0
The equivalent time is 0 hours, 0 minutes, and 0 seconds.
```

#### 示例Ⅱ

```
Please enter the number of seconds from midnight:
3723
The equivalent time is 1 hours, 2 minutes, and 3 seconds.
```

#### 示例Ⅲ

```
Please enter the number of seconds from midnight:
86400
The equivalent time is 24 hours, 0 minutes, and 0 seconds.
```

## 三

### 代码设计

> **simplify函数的设计不是很符合规范。原题目要求它返回一个fraction结构体，我懒得理它。**

```c
#include <stdio.h>
#include <math.h>

struct fraction
{
	int numerator;//分子
	int denominator;//分母
};

int max(int a,int b)
{
	return (a>b)?a:b;
}

int min(int a,int b)
{
	return (a<b)?a:b;
}

void simplify(struct fraction *p)
{
	int a=max(abs(p->denominator),abs(p->numerator));
	int b=min(abs(p->denominator),abs(p->numerator));
	if(p->denominator==0)
	{
		return;
	}
	if(p->numerator==0)
	{
		p->denominator/=abs(p->denominator);
		return;
	}
	int c;
	while(a%b!=0)
	{
		c=a%b;
		a=max(c,b);
		b=min(c,b);
	}
	if(p->denominator<0&&p->numerator<0)
	{
		b=0-b;
	}
	p->denominator/=b;
	p->numerator/=b;
}

struct fraction add(struct fraction f1,struct fraction f2)
{
	struct fraction f={f1.numerator*f2.denominator+f1.denominator*f2.numerator,f1.denominator*f2.denominator};
	simplify(&f);
	return f;
}

struct fraction sub(struct fraction f1,struct fraction f2)
{
	struct fraction f={f1.numerator*f2.denominator-f1.denominator*f2.numerator,f1.denominator*f2.denominator};
	simplify(&f);
	return f;
}

struct fraction time(struct fraction f1,struct fraction f2)
{
	struct fraction f={f1.numerator*f2.numerator,f1.denominator*f2.denominator};
	simplify(&f);
	return f;
}

struct fraction divide(struct fraction f1,struct fraction f2)
{
	struct fraction f={f1.numerator*f2.denominator,f1.denominator*f2.numerator};
	simplify(&f);
	return f;
}

int main()
{
	struct fraction f1,f2;
	while(1)
	{
		printf("Enter the numerator and denominator of the first fraction, separated by spaces:\n");
		scanf("%d %d",&f1.numerator,&f1.denominator);
		if(f1.denominator!=0)
		{
			simplify(&f1);
			printf("The simplest form of the first fraction is:%d/%d\n",f1.numerator,f1.denominator);
			break;
		}
		printf("Error, the denominator of the fraction should not be 0, please re-enter!\n");
	}
	while(1)
	{
		printf("Enter the numerator and denominator of the second fraction, separated by spaces:\n");
		scanf("%d %d",&f2.numerator,&f2.denominator);
		if(f2.denominator!=0)
		{
			simplify(&f2);
			printf("The simplest form of the second fraction is:%d/%d\n",f2.numerator,f2.denominator);
			break;
		}
		printf("Error, the denominator of the fraction should not be 0, please re-enter!\n");
	}
	printf("The sum of the two fractions is:%d/%d\n",add(f1,f2).numerator,add(f1,f2).denominator);
	printf("The difference between the two fractions is:%d/%d\n",sub(f1,f2).numerator,sub(f1,f2).denominator);
	printf("The product of the two fractions is:%d/%d\n",time(f1,f2).numerator,time(f1,f2).denominator);
	if(divide(f1,f2).denominator!=0)
	{
		printf("The quotient of the two fractions is:%d/%d\n",divide(f1,f2).numerator,divide(f1,f2).denominator);
	}
	else
	{
		printf("The quotient of these two fractions cannot be calculated because the divisor cannot be 0!\n");
	}
	return 0;
}
```

### 运行展示

#### 示例Ⅰ

```
Enter the numerator and denominator of the first fraction, separated by spaces:
8 12
The simplest form of the first fraction is:2/3
Enter the numerator and denominator of the second fraction, separated by spaces:
-35 63
The simplest form of the second fraction is:-5/9
The sum of the two fractions is:1/9
The difference between the two fractions is:11/9
The product of the two fractions is:-10/27
The quotient of the two fractions is:6/-5
```

#### 示例Ⅱ

```
Enter the numerator and denominator of the first fraction, separated by spaces:
-8 -2
The simplest form of the first fraction is:4/1
Enter the numerator and denominator of the second fraction, separated by spaces:
12 0
Error, the denominator of the fraction should not be 0, please re-enter!
Enter the numerator and denominator of the second fraction, separated by spaces:
0 114514
The simplest form of the second fraction is:0/1
The sum of the two fractions is:4/1
The difference between the two fractions is:4/1
The product of the two fractions is:0/1
The quotient of these two fractions cannot be calculated because the divisor cannot be 0!
```

#### 示例Ⅲ

```
Enter the numerator and denominator of the first fraction, separated by spaces:
55 66
The simplest form of the first fraction is:5/6
Enter the numerator and denominator of the second fraction, separated by spaces:
42 -144
The simplest form of the second fraction is:7/-24
The sum of the two fractions is:13/24
The difference between the two fractions is:9/8
The product of the two fractions is:35/-144
The quotient of the two fractions is:-20/7
```

## 四

### 代码设计

> 说真的为什么不用浮点数表示坐标啊，气死人了o(TヘTo)

```c
#include <stdio.h>

struct point
{
	int x;
	int y;
};

struct rectangle
{
	struct point upper_left;
	struct point lower_right;
};

long long int getarea(struct rectangle r)
{
	return ((long long int)r.upper_left.y-(long long int)r.lower_right.y)*((long long int)r.lower_right.x-(long long int)r.upper_left.x);
}

struct point getpoint(struct rectangle r)
{
	struct point p={(r.upper_left.x+r.lower_right.x)/2,(r.upper_left.y+r.lower_right.y)/2};
	return p;
}

struct rectangle move(struct rectangle r,double x,double y)
{
	struct rectangle rnew={{r.upper_left.x+x,r.upper_left.y+y},{r.lower_right.x+x,r.lower_right.y+y}};
	return rnew;
}

int ishere(struct rectangle r,struct point p)
{
	return (p.x>=r.upper_left.x&&p.x<=r.lower_right.x&&p.y>=r.lower_right.y&&p.y<=r.upper_left.y)?1:0;
}

int main()
{
	struct rectangle r;
	struct point p;
	int x,y;
	printf("Please enter the x,y coordinates in the upper left corner of the rectangle, separated by spaces:\n");
	scanf("%d %d",&r.upper_left.x,&r.upper_left.y);
	while(1)
	{
		printf("Please enter the x,y coordinates in the lower right corner of the rectangle, separated by spaces:\n");
		scanf("%d %d",&r.lower_right.x,&r.lower_right.y);
		if(r.lower_right.x>r.upper_left.x&&r.lower_right.y<r.upper_left.y)
		{
			break;
		}
		printf("Error, the input data cannot form a rectangle, please re-enter!\n");
	}
	printf("The area of the rectangle is:%lld\n",getarea(r));
	printf("The central coordinates of the rectangle are (which may be truncated):%d,%d\n",getpoint(r).x,getpoint(r).y);
	printf("Enter the coordinates of the point you want to find, separated by a space between the two data:\n");
	scanf("%d %d",&p.x,&p.y);
	if(ishere(r,p)==1)
	{
		printf("The point is inside the rectangle.\n");
	}
	else
	{
		printf("The point is not inside the rectangle.\n");
	}
	printf("Please enter the direction vector of the rectangle movement, separated by a space between the two data:\n");
	scanf("%d %d",&x,&y);
	printf("The coordinates of the upper left corner of the moved rectangle are %d,%d; The lower right corner coordinates are %d,%d.\n",move(r,x,y).upper_left.x,move(r,x,y).upper_left.y,move(r,x,y).lower_right.x,move(r,x,y).lower_right.y);
	return 0;
}
```

### 运行展示

#### 示例Ⅰ

```
Please enter the x,y coordinates in the upper left corner of the rectangle, separated by spaces:
0 271828
Please enter the x,y coordinates in the lower right corner of the rectangle, separated by spaces:
314159 0
The area of the rectangle is:85397212652
The central coordinates of the rectangle are (which may be truncated):157079,135914
Enter the coordinates of the point you want to find, separated by a space between the two data:
114514 114514
The point is inside the rectangle.
Please enter the direction vector of the rectangle movement, separated by a space between the two data:
23579 -23579
The coordinates of the upper left corner of the moved rectangle are 23579,248249; The lower right corner coordinates are 337738,-23579.
```

#### 示例Ⅱ

```
Please enter the x,y coordinates in the upper left corner of the rectangle, separated by spaces:
10 0
Please enter the x,y coordinates in the lower right corner of the rectangle, separated by spaces:
0 10
Error, the input data cannot form a rectangle, please re-enter!
Please enter the x,y coordinates in the lower right corner of the rectangle, separated by spaces:
16 -8
The area of the rectangle is:48
The central coordinates of the rectangle are (which may be truncated):13,-4
Enter the coordinates of the point you want to find, separated by a space between the two data:
0 0
The point is not inside the rectangle.
Please enter the direction vector of the rectangle movement, separated by a space between the two data:
0 -10
The coordinates of the upper left corner of the moved rectangle are 10,-10; The lower right corner coordinates are 16,-18.
```

#### 示例Ⅲ

```
Please enter the x,y coordinates in the upper left corner of the rectangle, separated by spaces:
-10 10
Please enter the x,y coordinates in the lower right corner of the rectangle, separated by spaces:
10 -10
The area of the rectangle is:400
The central coordinates of the rectangle are (which may be truncated):0,0
Enter the coordinates of the point you want to find, separated by a space between the two data:
20 0
The point is not inside the rectangle.
Please enter the direction vector of the rectangle movement, separated by a space between the two data:
3 -2
The coordinates of the upper left corner of the moved rectangle are -7,8; The lower right corner coordinates are 13,-12.
```

