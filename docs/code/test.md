<script>
    locker("Alhaitham");
</script>



# 期末复习题

## 免责声明及警告

以下代码仅作参考，不保证准确性、鲁棒性或最优性，也没有任何人应当或可能为此负责。公布代码仅用于相互交流、讨论或学习，不得用于任何违法、违规或违纪之用途；违者自行承担全部责任和损失，自行为自己的前途负责。

代码尽力为所有题目提供容易被理解的解答。若有疏漏之处或其它意见，欢迎讨论。

## 一

```c
#include <stdio.h>
#include <string.h>

void print(int a,int b,char s[])
{
    for(int i=b;i>=a;i--)
    {
        printf("%c",s[i]);
    }
}

int main()
{
    int n=0;
    scanf("%d\n",&n);
    char str[512],w[512];
    for(int i=0;i<n;i++)
    {
        gets(str);
        gets(w);
        printf("%d\n",(int)(strstr(str,w)-str)-1);
    }
    return 0;
}
```

## 二

```c
#include <stdio.h>
#include <string.h>

int gen(int n)
{
    if(n==1)
    {
        return 2;
    }
    else if(n==2)
    {
        return 3;
    }
    else
    {
        return gen(n-1)+gen(n-2);
    }
}

int main()
{
    int n=0,ans=1,b=0,e=0;
    int list[256];
    scanf("%d\n", &n);
    char str[256];
    for(int i=0;i<n;i++)
    {
        ans=1;
        gets(str);
        for(int j=0;str[j+1]!='\0';j++)
        {
            if((str[j]-'0')*10+(str[j+1]-'0')<=26&&str[j]!='0'&&str[j+1]!='0')
            {
                list[j]=1;
            }
            else
            {
                list[j]=0;
            }
        }
        list[strlen(str)-1]=0;
        for(int j=0;str[j+1]!='\0';j++)
        {
            if((list[j]==1&&j==0)||(list[j]==1&&list[j-1]==0&&j!=0))
            {
                b=j;
            }
            if(list[j]==1&&list[j+1]==0)
            {
                e=j;
                //printf("b=%d e=%d\n",b,e);
                ans*=gen(e-b+1);
            }
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

## 三

```c
#include <stdio.h>
#include <string.h>

int list[128];
int selist[128];

struct student
{
    char name[100];
    int id;
    int score;
};

struct student s[128];

void rank(int be,int en)
{
    //printf("b=%d,%d",be,en);
    char min[128]="~",tar[128]=" ";
    for(int j=be;j<=en;j++)
    {
        int index=0;
        for(int i=be;i<=en;i++)
        {
            if(strcmp(s[list[i]].name,min)<0&&strcmp(s[list[i]].name,tar)>0)
            {
                strcpy(min,s[list[i]].name);
                index=list[i];
            }
        }
        selist[j]=index;
        strcpy(min,"~");
        strcpy(tar,s[index].name);
    }
    for(int j=be;j<=en;j++)
    {
        list[j]=selist[j];
    }
}

int main()
{
    int n=0;
    scanf("%d",&n);
    int a=0,b=0,c=0;
    for(int i=0;i<n;i++)
    {
        scanf("\n%s %d %d %d %d",s[i].name,&s[i].id,&a,&b,&c);
        s[i].score=a+b+c;
    }
    int max=0,index=0,num=0,be=-1,en=-1;
    for(int j=0;j<n;j++)
    {
        max=0;
        for(int i=0;i<n;i++)
        {
            if(s[i].score>=max)
            {
                max=s[i].score;
                index=i;
            }
        }
        list[num]=index;
        num++;
        s[index].score=0-s[index].score;
    }
    for(int j=0;j<n;j++)
    {
        //printf("%d]\n",list[j]);
    }
    for(int j=0;j<n;j++)
    {
        if(be==-1&&j!=n-1&&s[list[j]].score==s[list[j+1]].score)
        {
            be=j;
        }
        if(en==-1&&j!=0&&s[list[j]].score==s[list[j-1]].score&&(j==n-1||s[list[j]].score!=s[list[j+1]].score))
        {
            en=j;
            rank(be,en);
            be=-1;
            en=-1;
        }
    }
    for(int j=0;j<n;j++)
    {
        printf("[name:%s,sum:%d,sno:%d]\n",s[list[j]].name,0-s[list[j]].score,s[list[j]].id);
    }
    return 0;
}
```

## 四

```c
#include <stdio.h>
#include <math.h>

int main()
{
    int n=0,f=0;
    scanf("%d", &n);
    for(int i=2;i<=n;i++)
    {
        f=0;
        for(int j=2;j<=sqrt(i);j++)
        {
            if(i%j==0)
            {
                f=1;
                break;
            }
        }
        if(f==0)
        {
            printf("%d ", i);
        }
    }
    return 0;
}
```

## 五

```c
#include <stdio.h>
#include <string.h>

int list[128];
int selist[128];

struct student
{
    char name[100];
    int id;
    int score;
};

struct student s[128];

void rank(int be,int en)
{
    //printf("b=%d,%d",be,en);
    char min[128]="~",tar[128]=" ";
    for(int j=be;j<=en;j++)
    {
        int index=0;
        for(int i=be;i<=en;i++)
        {
            if(strcmp(s[list[i]].name,min)<0&&strcmp(s[list[i]].name,tar)>0)
            {
                strcpy(min,s[list[i]].name);
                index=list[i];
            }
        }
        selist[j]=index;
        strcpy(min,"~");
        strcpy(tar,s[index].name);
    }
    for(int j=be;j<=en;j++)
    {
        list[j]=selist[j];
    }
}

int main()
{
    int a=0,b=0,c=0;
    int n=0;
    for(int i=0;1;i++)
    {
        scanf("\n%s",s[i].name);
        if(strcmp(s[i].name,"exit")==0)
        {
            n=i;
            break;
        }
        scanf(" %d %d %d %d",&s[i].id,&a,&b,&c);
        s[i].score=a+b+c;
    }
    int max=0,index=0,num=0,be=-1,en=-1;
    for(int j=0;j<n;j++)
    {
        max=0;
        for(int i=0;i<n;i++)
        {
            if(s[i].score>=max)
            {
                max=s[i].score;
                index=i;
            }
        }
        list[num]=index;
        num++;
        s[index].score=0-s[index].score;
    }
    for(int j=0;j<n;j++)
    {
        //printf("%d]\n",list[j]);
    }
    for(int j=0;j<n;j++)
    {
        if(be==-1&&j!=n-1&&s[list[j]].score==s[list[j+1]].score)
        {
            be=j;
        }
        if(en==-1&&j!=0&&s[list[j]].score==s[list[j-1]].score&&(j==n-1||s[list[j]].score!=s[list[j+1]].score))
        {
            en=j;
            rank(be,en);
            be=-1;
            en=-1;
        }
    }
    printf("[");
    for(int j=0;j<n;j++)
    {
        printf("{name:%s,sum:%d,sno:%d}",s[list[j]].name,0-s[list[j]].score,s[list[j]].id);
        if(j!=n-1)
        {
            printf(",\n");
        }
        else
        {
            printf("]");
        }
    }
    return 0;
}
```

## 六

```c
#include <stdio.h>
#include <string.h>

int main()
{
    int re[4]={0},f=0,rank[3]={0};
    char n[3][128],c[128]=" ";
    int s;
    for(int i=0;i<3;i++)
    {
        scanf("%s",n[i]);
    }
    scanf("%s",c);
    while(strcmp(c,"END")!=0)
    {
        f=0;
        for(int i=0;i<3;i++)
        {
            if(strcmp(c,n[i])==0)
            {
                re[i]++;
                f=1;
            }
        }
        if(f==0)
        {
            re[3]++;
        }
        scanf("%s",c);
    }
    char tar[20]=" ",min[20]="~";
    int index=0;
    for(int i=0;i<3;i++)
    {
        for(int j=0;j<3;j++)
        {
            if(strcmp(n[j],min)<0&&strcmp(n[j],tar)>0)
            {
                strcpy(min,n[j]);
                index=j;
            }
        }
        rank[i]=index;
        strcpy(tar,min);
        strcpy(min,"~");
    }
    for(int i=0;i<3;i++)
    {
        printf("%s:%d\n",n[rank[i]],re[i]);
    }
    printf("QQ:%d\n",re[3]);
    return 0;
}
```

## 七

```c
#include <stdio.h>
#include <string.h>

int list[128];
int selist[128];

struct student
{
    char name[100];
    int id;
    int score;
};

struct student s[128];

void rank(int be,int en)
{
    //printf("b=%d,%d",be,en);
    int min=999999,tar=0;
    for(int j=be;j<=en;j++)
    {
        int index=0;
        for(int i=be;i<=en;i++)
        {
            if(s[list[i]].id<min&&s[list[i]].id>tar)
            {
                min=s[list[i]].id;
                index=list[i];
            }
        }
        selist[j]=index;
        min=999999;
        tar=s[index].id;
    }
    for(int j=be;j<=en;j++)
    {
        list[j]=selist[j];
    }
}

int main()
{
    int n=0;
    scanf("%d",&n);
    int a=0,b=0;
    for(int i=0;i<n;i++)
    {
        scanf("\n%s %d %d %d",s[i].name,&s[i].id,&a,&b);
        s[i].score=a+b;
    }
    int max=0,index=0,num=0,be=-1,en=-1;
    for(int j=0;j<n;j++)
    {
        max=0;
        for(int i=0;i<n;i++)
        {
            if(s[i].score>=max)
            {
                max=s[i].score;
                index=i;
            }
        }
        list[num]=index;
        num++;
        s[index].score=0-s[index].score;
    }
    for(int j=0;j<n;j++)
    {
        //printf("%d]\n",list[j]);
    }
    for(int j=0;j<n;j++)
    {
        if(be==-1&&j!=n-1&&s[list[j]].score==s[list[j+1]].score)
        {
            be=j;
        }
        if(en==-1&&j!=0&&s[list[j]].score==s[list[j-1]].score&&(j==n-1||s[list[j]].score!=s[list[j+1]].score))
        {
            en=j;
            rank(be,en);
            be=-1;
            en=-1;
        }
    }
    for(int j=0;j<n;j++)
    {
        printf("%s %d\n",s[list[j]].name,0-s[list[j]].score);
    }
    return 0;
}
```

## 八

```c
#include <stdio.h>
#include <string.h>

int main()
{
    char n[5][2048];
    for(int i=0;i<4;i++)
    {
        gets(n[i]);
    }
    gets(n[4]);
    for(int i=0;i<4;i++)
    {
        for(int j=i+1;j<4;j++)
        {
            if(strcmp(n[i],n[j])>0)
            {
                char temp[2048];
                strcpy(temp,n[i]);
                strcpy(n[i],n[j]);
                strcpy(n[j],temp);
            }
        }
    }
    for(int i=0;i<4;i++)
    {
        printf("%s\n",n[i]);
    }
    for(int i=0;i<5;i++)
    {
        for(int j=i+1;j<5;j++)
        {
            if(strcmp(n[i],n[j])>0)
            {
                char temp[2048];
                strcpy(temp,n[i]);
                strcpy(n[i],n[j]);
                strcpy(n[j],temp);
            }
        }
    }
    printf("\n");
    for(int i=0;i<5;i++)
    {
        printf("%s\n",n[i]);
    }
    return 0;
}
```

## 九

```c
#include <stdio.h>
#include <ctype.h>
#include <stdlib.h>

int main()
{
    char or[512],a[256],b[256],c;
    gets(or);
    int f=0,i=0;
    if(isdigit(or[i])==0)
    {
        f=1;
    }
    while(isdigit(or[i])!=0||or[i]=='.')
    {
        a[i]=or[i];
        i++;
    }
    a[i]='\0';
    if(or[i]!='+'&&or[i]!='-'&&or[i]!='*'&&or[i]!='/')
    {
        f=1;
    }
    else
    {
        c=or[i];
        i++;
    }
    if(isdigit(or[i])==0)
    {
        f=1;
    }
    int bi=i;
    while(isdigit(or[i])!=0||or[i]=='.')
    {
        b[i-bi]=or[i];
        i++;
    }
    b[i]='\0';
    if(or[i]!='\0')
    {
        f=1;
    }
    if(f!=0)
    {
        printf("Input Error");
    }
    else
    {
        double ca=atof(a);
        double cb=atof(b);
        if(c=='+')
        {
            printf("%.2f",ca+cb);
        }
        else if(c=='-')
        {
            printf("%.2f",ca-cb);
        }
        else if(c=='*')
        {
            printf("%.2f",ca*cb);
        }
        else if(c=='/')
        {
            if(cb!=0)
            {
                printf("%.2f",ca/cb);
            }
            else
            {
                printf("Input Error");
            }
            
        }
    }
    return 0;
}
```

## 十

```c
#include <stdio.h>

int main()
{
    int n=0,l[512];
    while(scanf("%d",&l[n])!=EOF)
    {
        n++;
    }
    for(int i=0;i<n-1;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            if(l[i]<l[j])
            {
                int t=l[i];
                l[i]=l[j];
                l[j]=t;
            }
        }
    }
    printf("%d",l[0]);
    return 0;
}
```

## 十一

```c
#include <stdio.h>

int main()
{
    int n=0,l,ans=0;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&l);
        ans=ans^l;
    }
    printf("%d",ans);
    return 0;
}
```

## 十二

```c
#include <stdio.h>
#include <string.h>

int list[128];

struct student
{
    char name[100];
    int score;
};

struct student s[128];

int main()
{
    int n=0;
    scanf("%d",&n);
    int a=0,b=0,c=0;
    for(int i=0;i<n;i++)
    {
        scanf("\n%s %d %d %d",s[i].name,&a,&b,&c);
        s[i].score=a+b+c;
        printf("%.2f\n",(s[i].score)/3.0);
    }
    int max=0,index=0,num=0,be=-1,en=-1;
    for(int j=0;j<n;j++)
    {
        max=0;
        for(int i=0;i<n;i++)
        {
            if(s[i].score>=max)
            {
                max=s[i].score;
                index=i;
            }
        }
        list[num]=index;
        num++;
        s[index].score=0-s[index].score;
    }
    for(int j=0;j<n;j++)
    {
        //printf("%d]\n",list[j]);
    }
    int co=0;
    do{
        //printf("%s %.2f\n",s[list[co]].name,(0-s[list[co]].score)/3.0);
        co++;
    }while(s[list[co]].score==s[list[co-1]].score&&co<n);
    for(int i=co-1;i>=0;i--)
    {
        printf("%s %.2f\n",s[list[i]].name,(0-s[list[i]].score)/3.0);
    }
    return 0;
}
```

## 十三

```c
#include <stdio.h>
#include <string.h>

void print(int a,int b,char s[])
{
    for(int i=b;i>=a;i--)
    {
        printf("%c",s[i]);
    }
}

int main()
{
    char str[512];
    gets(str);
    int n=strlen(str);
    int b=0,e=0;
    for(int i=0;i<n;i++)
    {
        if(i!=0&&str[i]!=' '&&str[i-1]==' ')
        {
            b=i;
        }
        if(i==n-1&&str[i]!=' '||str[i]!=' '&&str[i+1]==' ')
        {
            e=i;
            print(b,e,str);
        }
        if(str[i]==' ')
        {
            printf("%c",str[i]);
        }
    }
    return 0;
}
```

## 十四

```c
#include <stdio.h>

int main()
{
    double m=0,l[12];
    scanf("%lf",&m);
    for(int i=0;i<12;i++)
    {
        scanf("%lf",&l[i]);
    }
    for(int i=0;i<11;i++)
    {
        for(int j=i+1;j<12;j++)
        {
            if(l[i]<l[j])
            {
                double t=l[i];
                l[i]=l[j];
                l[j]=t;
            }
        }
    }
    printf("%.2f",m*l[0]);
    return 0;
}
```

## 十五

```c
#include <stdio.h>

int main()
{
    int n=0,c=0;
    scanf("%d", &n);
    for(int i=0;5*i<=n;i++)
    {
        for(int j=0;3*j+5*i<=n;j++)
        {
            for(int k=0;3*j+5*i+k/3<=n;k+=3)
            {
                if(3*j+5*i+k/3==n&&i+j+k==n)
                {
                    printf("%d %d %d\n", i, j, k);
                    c++;
                }
            }
        }
    }
    if(c==0)
    {
        printf("NO\n");
    }
    return 0;
}
```

## 十六

```c
#include <stdio.h>

int main()
{
    int n=0,c=0;
    int num[10];
    scanf("%d", &n);
    while(n!=0)
    {
        num[c]=n%10;
        n=n/10;
        c++;
    }
    printf("%d\n", c);
    for(int i=c-1;i>=0;i--)
    {
        printf("%d ", num[i]);
    }
    printf("\n");
    for(int i=0;i<c;i++)
    {
        printf("%d ", num[i]);
    }
    return 0;
}
```

## 十七

```c
#include <stdio.h>

int main()
{
    int n=0;
    scanf("%d", &n);
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=i;j++)
        {
            printf("%d*%d=%-3d", i, j, i*j);
        }
        printf("\n");
    }
    return 0;
}
```

## 十八

```c
#include <stdio.h>
#include <string.h>

int main()
{
    char str[512];
    gets(str);
    int n=strlen(str);
    for(int i=0;i<n;i++)
    {
        if(str[i]>='a'&&str[i]<='z')
        {
            str[i]=((int)str[i]+4-'a')%26+'a';
        }
        else if(str[i]>='A'&&str[i]<='Z')
        {
            str[i]=((int)str[i]+4-'A')%26+'A';
        }
    }
    printf("%s\n",str);
    return 0;
}
```

## 十九

```c
#include <stdio.h>
#include <string.h>

int main()
{
    int rank[10];
    for(int i=0;i<10;i++)
    {
        scanf("%d",&rank[i]);
    }
    int min=999999,max=-999999;
    int ii=0,ai=0;
    for(int i=0;i<10;i++)
    {
        if(rank[i]<min)
        {
            min=rank[i];
            ii=i;
        }
    }
    for(int i=0;i<10;i++)
    {
        if(rank[i]>max)
        {
            max=rank[i];
            ai=i;
        }
    }
    rank[0]=rank[0]+rank[ii];
    rank[ii]=rank[0]-rank[ii];
    rank[0]=rank[0]-rank[ii];
    rank[9]=rank[9]+rank[ai];
    rank[ai]=rank[9]-rank[ai];
    rank[9]=rank[9]-rank[ai];
    for(int i=0;i<10;i++)
    {
        printf("%d ",rank[i]);
    }
    return 0;
}
```

## 二十

```c
#include <stdio.h>
#include <string.h>

int main()
{
    int n=0,l=0;
    int rank[512];
    scanf("%d %d",&n,&l);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&rank[i]);
    }
    for(int i=0;i<n;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            if(rank[i]<rank[j])
            {
                int temp=rank[i];
                rank[i]=rank[j];
                rank[j]=temp;
            }
        }
    }
    for(int i=0;i<l;i++)
    {
        printf("%d ",rank[i]);
    }
    return 0;
}
```

## 二十一

```c
#include <stdio.h>
#include <string.h>

int main()
{
    int n=0,l=0,k=0,y=0,count=0;
    scanf("%d %d %d %d",&n,&l,&k,&y);
    char list[512];
    int rank[512];
    for(int i=0;i<n;i++)
    {
        count=0;
        scanf("%s",list);
        for(int j=0;list[j]!='\0';j++)
        {
            if(list[j]=='A')
            {
                count+=l+k+y;
            }
            else if(list[j]=='B')
            {
                count+=2*l+k;
            }
            else if(list[j]=='C')
            {
                count+=3*l+3*k+2*y;
            }
        }
        rank[i]=count;
    }
    for(int i=0;i<n;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            if(rank[i]>rank[j])
            {
                int temp=rank[i];
                rank[i]=rank[j];
                rank[j]=temp;
            }
        }
    }
    printf("%d",rank[0]);
    return 0;
}
```

## 二十二

```c
#include <stdio.h>

int main()
{
    int n=0;
    int map[16][16];
    scanf("%d", &n);
    for(int i=0;i<n;i++)
    {
        map[i][0]=1;
        printf("%-4d",map[i][0]);
        for(int j=1;j<i;j++)
        {
            map[i][j]=map[i-1][j-1]+map[i-1][j];
            printf("%-4d",map[i][j]);
        }
        if(i!=0)
        {
            map[i][i]=1;
            printf("%-4d",map[i][i]);
        }
        printf("\n");
    }
    return 0;
}
```

## 二十三

```c
#include <stdio.h>

int main()
{
    int n=0,x=0,y=0;
    int map[100][100]={0};
    scanf("%d", &n);
    map[0][0]=1;
    for(int i=2;i<=n*n;)
    {
        while(map[x+1][y]==0&&x+1<n)
        {
            map[x+1][y]=i;
            x+=1;
            i++;
        }
        while(map[x][y+1]==0&&y+1<n)
        {
            map[x][y+1]=i;
            y+=1;
            i++;
        }
        while(x>0&&map[x-1][y]==0)
        {
            map[x-1][y]=i;
            x-=1;
            i++;
        }
        while(y>0&&map[x][y-1]==0)
        {
            map[x][y-1]=i;
            y-=1;
            i++;
        }
    }
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            printf("%-5d", map[j][i]);
        }
        printf("\n");
    }
    return 0;
}
```

## 二十四

```c
#include <stdio.h>

int main()
{
    while(1)
    {
        int set[10],te[10],f=0;
        for(int i=0;i<5;i++)
        {
            scanf("%d",&set[i]);
        }
        for(int i=0;i<5;i++)
        {
            if(set[i]!=0)
            {
                f=1;
                break;
            }
        }
        if(f==0)
        {
            break;
        }
        te[0]=set[0];
        te[1]=set[1];
        f=0;
        for(int i=2;i<5;i++)
        {
            te[i]=te[i-1]+(te[1]-te[0]);
            if(te[i]!=set[i])
            {
                f=1;
                break;
            }
        }
        if(f==0)
        {
            printf("case one\n");
            for(int i=5;i<10;i++)
            {
                te[i]=te[i-1]+(te[1]-te[0]);
                printf("%d ",te[i]);
            }
            printf("\n");
        }
        f=0;
        for(int i=2;i<5;i++)
        {
            te[i]=te[i-1]*(te[1]/te[0]);
            if(te[i]!=set[i])
            {
                f=1;
                break;
            }
        }
        if(f==0)
        {
            printf("case two\n");
            for(int i=5;i<10;i++)
            {
                te[i]=te[i-1]*(te[1]/te[0]);
                printf("%d ",te[i]);
            }
            printf("\n");
        }
        f=0;
        for(int i=2;i<5;i++)
        {
            te[i]=te[i-1]+te[i-2];
            if(te[i]!=set[i])
            {
                f=1;
                break;
            }
        }
        if(f==0)
        {
            printf("case three\n");
            for(int i=5;i<10;i++)
            {
                te[i]=te[i-1]+te[i-2];
                printf("%d ",te[i]);
            }
            printf("\n");
        }
    }
    return 0;
}
```

## 二十五

```c
#include <stdio.h>
#include <string.h>

int main()
{
    char str[512];
    gets(str);
    int n=strlen(str);
    char target[512]=" ";
    char min[512]="~";
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            if(strcmp(str+j,min)<0&&strcmp(str+j,target)>0)
            {
                strcpy(min,str+j);
            }
        }
        printf("%s\n",min);
        strcpy(target,min);
        strcpy(min,"~");
    }
    return 0;
}
```

## 二十六

```c
#include <stdio.h>

int main()
{
    int n=0;
    char f;
    scanf("%c %d",&f,&n);
    for(int i=0;i<n;i++)
    {
        if(i==n-1)
        {
            for(int j=0;j<2*n-1;j++)
            {
                printf("%c",f);
            }
            printf("\n");
            break;
        }
        for(int j=0;j<n-i-1;j++)
        {
            printf("*");
        }
        printf("%c",f);
        for(int j=0;j<2*i-1;j++)
        {
            printf("*");
        }
        if(i!=0)
        {
            printf("%c",f);
        }
        for(int j=0;j<n-i-1;j++)
        {
            printf("*");
        }
        printf("\n");
    }
    return 0;
}
```

## 二十七

```c
#include <stdio.h>
#include <math.h>

int gen(int s,int c)
{
    int n=s,i=0;
    int ans=0;
    int d[100];
    while(n!=0)
    {
        d[i]=n-(n/10)*10;
        i++;
        n=n/10;
    }
    if(c%2==0)
    {
        for(int j=0;j<i;j++)
        {
            ans+=d[j]*(pow(10,i-1-j)+pow(10,i+j));
        }
    }
    else
    {
        for(int j=0;j<i;j++)
        {
            ans+=d[j]*(pow(10,i-1-j)+pow(10,i+j-1));
        }
        ans-=d[0]*pow(10,i-1);
    }
    return ans;
}

int main()
{
    int t=1;
    while(t!=0)
    {
        scanf("%d",&t);
        if(t!=0)
        {
            int n=t,c=0;
            while(n!=0)
            {
                n=n/10;
                c++;
            }
            int min=pow(10,(c+1)/2-1);
            while(gen(min,c)<=t)
            {
                min++;
            }
            printf("%d\n",gen(min,c));
        }
    }
    return 0;
}
```

## 二十八

```c
#include <stdio.h>

int set[100];
int se[10];
int n=0;

void r(int p,int l)
{
    se[l]=p;
    if(l==6)
    {
        for(int j=1;j<=6;j++)
        {
            printf("%d ",set[se[j]]);
        }
        printf("\n");
        return;
    }
    for(int i=p+1;i<n;i++)
    {
        r(i,l+1);
    }
}

int main()
{
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&set[i]);
    }
    for(int i=0;i<n-1;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            if(set[i]>set[j])
            {
                int temp=set[i];
                set[i]=set[j];
                set[j]=temp;
            }
        }
    }
    for(int i=0;i+5<n;i++)
    {
        r(i,1);
    }
    return 0;
}
```

## 二十九

```c
#include <stdio.h>

int s(int n)
{
    if(n==1)
        return 1;
    else if(n==2)
        return 2;
    else
        return s(n-1)+s(n-2);
}

int main()
{
    int n=0;
    scanf("%d", &n);
    printf("%d",s(n));
    return 0;
}
```

## 三十

```c
#include <stdio.h>

int main()
{
    char str[512000];
    gets(str);
    int n=0;
    int s=0,d=0,x=0,z=0;
    while(str[n]!='\0')
    {
        if(str[n]>='0'&&str[n]<='9')
            s++;
        else if(str[n]>='a'&&str[n]<='z')
            x++;
        else if(str[n]>='A'&&str[n]<='Z')
            d++;
        else
            z++;
        n++;
    }
    printf("%d %d %d %d\n",s,d,x,z);
    return 0;
}
```

## 三十一

```c
#include <stdio.h>

int main()
{
    int n=0;
    int a[100][100],b[100][100];
    scanf("%d", &n);
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            scanf("%d", &a[i][j]);
        }
    }
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            b[i][j] = a[i][j] + a[j][i];
        }
    }
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            printf("%d ", b[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```

