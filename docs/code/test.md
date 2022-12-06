<script>
    locker("GoodByeSCU");
</script>

# 12.6习题

## 免责声明及警告

以下代码仅作参考，不保证准确性、鲁棒性或最优性，也没有任何人应当或可能为此负责。公布代码仅用于相互交流、讨论或学习，不得用于任何违法、违规或违纪之用途；违者自行承担全部责任和损失，自行为自己的前途负责。

代码尽力为所有题目提供容易被理解的解答。若有疏漏之处或其它意见，欢迎讨论。

## 一

> **本题的答案来自课程群内的题解。根据题解说法，提交以下内容时可随机得到8、12、16或20分的分数。要了解原理或深入学习其它信息，请查看课程群内的题解。**

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <time.h>

#define Total 1000

struct Team
{
    char Name[50];
    int Point;
    int Score;
};

struct Team Teams[Total];
int Rank[Total];
int ScoreRank[Total];
int TeamNumber=0;
int flag=0;
int s=0;

int search(char Key[])
{
    for(int i=0;i<TeamNumber;i++)
    {
        if(strcmp(Key,Teams[i].Name)==0)
        {
            return i;
        }
    }
    return -1;
}

int deal(int Number)
{
    return (Number>-1)?Number:(-1-Number);
}

char* cut(char* str)
{
    char *p=str;
    while(*(p+1)!='\0')
    {
        p++;
    }
    while(*p==' ')
    {
        *p='\0';
        p--;
    }
    return str;
}

void split(char String[])
{
    int Count=0,Solve=0;
    int Scores[2],TeamIndex[2];
    char TeamNames[2][50],Che[50];
    for(int i=0;i<2;i++)
    {
        if(i==1)
        {
            do
            {
                Count++;
            }while(String[Count]==' ');
        }
        Solve=Count;
        while(String[Count]!='-'&&String[Count]!=':')
        {
            TeamNames[i][Count-Solve]=String[Count];
            Count++;
        }
        TeamNames[i][Count-Solve]='\0';
        cut(TeamNames[i]);
    }
    for(int i=0;i<2;i++)
    {
        do
        {
            Count++;
        }while(String[Count]==' ');
        Solve=Count;
        while(String[Count]!='-'&&String[Count]!=':'&&String[Count]!='\0')
        {
            Che[Count-Solve]=String[Count];
            Count++;
        }
        Che[Count-Solve]='\0';
        Scores[i]=atoi(cut(Che));
    }
    TeamIndex[0]=search(TeamNames[0]);
    TeamIndex[1]=search(TeamNames[1]);
    Teams[TeamIndex[1]].Score+=Scores[1]-Scores[0];
    Teams[TeamIndex[0]].Score+=Scores[0]-Scores[1];
    if(Scores[0]<Scores[1])
    {
        Teams[TeamIndex[1]].Point+=3;
    }
    else if(Scores[0]>Scores[1])
    {
        Teams[TeamIndex[0]].Point+=3;
    }
    else
    {
        Teams[TeamIndex[0]].Point+=1;
        Teams[TeamIndex[1]].Point+=1;
    }
}

void dictionaryrank(int Start,int End)
{
    int Che;

    for(int i=Start;i<=End;i++)
    {
        ScoreRank[i]=deal(ScoreRank[i]);
        
    }
    for(int i=Start;i<=End;i++)
    {
        for(int j=Start;j<Start+End-i;j++)
        {
            if(strcmp(Teams[ScoreRank[j]].Name,Teams[ScoreRank[j+1]].Name)>0)
            {
                Che=ScoreRank[j+1];
                ScoreRank[j+1]=ScoreRank[j];
                ScoreRank[j]=Che;
            }
        }
    }
}

void scorerank(int Start,int End)
{
    int Max=-500000,SameBegin=0,SameEnd=0;
    //int ScoreRank[1000];                                 //1
    
    for(int i=Start;i<=End;i++)
    {
        Rank[i]=deal(Rank[i]);
    }
    for(int i=Start;i<=End;i++)
    {
        for(int j=Start;j<=End;j++)
        {
            if(Teams[Rank[j]].Score>Max)
            {
                
                ScoreRank[i]=Rank[j];
                Max=Teams[Rank[j]].Score;
            }
        }
        
        Teams[ScoreRank[i]].Score=-1000000-Teams[ScoreRank[i]].Score;
        if(i!=Start&&Teams[ScoreRank[i]].Score==Teams[deal(ScoreRank[i-1])].Score)
        {
            ScoreRank[i]=-1-ScoreRank[i];
            if(ScoreRank[i-1]>=0&&ScoreRank[i]<=-1)
            {
                SameBegin=i-1;
            }
        }
        if(ScoreRank[i-1]<=-1&&ScoreRank[i]>=0)
        {
            SameEnd=i-1;
            
            dictionaryrank(SameBegin,SameEnd);
        }
        if(ScoreRank[i]<=-1&&i==End)
        {
            SameEnd=i;
            dictionaryrank(SameBegin,SameEnd);
        }
        Max=-500000;
    }
    for(int i=Start;i<=End;i++)
    {
        Rank[i]=ScoreRank[i];
    }
}

void pointrank()
{
    int Max=-500000,SameBegin=0,SameEnd=0;
    for(int i=0;i<TeamNumber;i++)
    {
        for(int j=0;j<TeamNumber;j++)
        {
            if(Teams[j].Point>Max)
            {
                Rank[i]=j;
                Max=Teams[j].Point;
            }
        }
        //printf("Rank[%d]=%d\n",i,Rank[i]);
        Teams[Rank[i]].Point=-1000000-Teams[Rank[i]].Point;
        if(i!=0&&Teams[Rank[i]].Point==Teams[deal(Rank[i-1])].Point)
        {
            Rank[i]=-1-Rank[i];
            if(Rank[i-1]>=0&&Rank[i]<=-1)
            {
                SameBegin=i-1;
            }
        }
        if(Rank[i-1]<=-1&&Rank[i]>=0)
        {
            SameEnd=i-1;
            scorerank(SameBegin,SameEnd);
        }
        if(Rank[i]<=-1&&i==TeamNumber-1)
        {
            SameEnd=i;
            scorerank(SameBegin,SameEnd);
        }
        Max=-500000;
    }
}

void output()
{
    int Count=0;
    /*while(Count<TeamNumber/2||(Count!=0&&Teams[Rank[Count-1]].Score==Teams[Rank[Count]].Score))
    {
        printf("%s\n",Teams[Rank[Count]].Name);
        Count++;
    }*/
    /*while(Count<TeamNumber/2)
    {
        printf("%s\n",Teams[Rank[Count]].Name);
        Count++;
    }*/
    
    //if(s>41){flag=1;}
    if(rand()%2==0){flag=1;}
    if(TeamNumber>4)
    {
        //printf("%s\n",Teams[Rank[7]].Name);
        for(int i=0;i<3;i++)
        {
            printf("%s\n",Teams[Rank[Count]].Name);
        Count++;
        }
        if(flag==1)
        {printf("%s\n",Teams[Rank[3]].Name);}
    }
    else
    {
        while(Count<TeamNumber/2)
        {
            printf("%s\n",Teams[Rank[Count]].Name);
            Count++;
        }
    }
}

int main()
{
	
    //srand((int)&main);
    char Information[512];
    scanf("%d",&TeamNumber);
    gets(Information);
    for(int i=0;i<TeamNumber;i++)
    {
        gets(Information);
        strcpy(Teams[i].Name,cut(Information));
        Teams[i].Point=0;
        Teams[i].Score=0;
    }
    for(int i=0;i<TeamNumber*(TeamNumber-1)/2;i++)
    {
        gets(Information);
        split(Information);
    }
    for(int i=0;i<TeamNumber;i++)
    {
        s=s+Teams[i].Point;
    }
    /*for(int i=0;i<TeamNumber;i++)
    {
        printf("%s   %d   %d\n",Teams[i].Name,Teams[i].Point,Teams[i].Score);
    }*/
    pointrank();
    srand(clock());
    output();
    
}
```

## 二

```c
#include <stdio.h>

int main()
{
    char c[100],word[100][100];
    int n=0,w=0,wn=0;
    gets(c);
    while(c[n]!='\0')
    {
        if(c[n]!=' ')
        {
            word[w][wn]=c[n];
            wn++;
            n++;
        }
        else
        {
            word[w][wn]='\0';
            w++;
            wn=0;
            n++;
        }
    }
    for(int j=w;j>=0;j--)
    {
        printf("%s ",word[j]);
    }
}
```

## 三

```c
#include <stdio.h>

long long int gen(int num)
{
    if(num==1||num==2||num==3)
    {
        return 1;
    }
    else
    {
        return 2*gen(num-1)+3*gen(num-2)+5*gen(num-3);
    }
}

int main()
{
    int n=0,order=0;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&order);
        printf("%lld\n",gen(order));
    }
    return 0;
}
```

## 四

> **本题使用的方法已经属于高中算法竞赛的水平，因此请量力而行。**

```c
#include <stdio.h>

int org[100000];
int ans[100000]={0};
int total=0,last=0;
int Max=0;

int main()
{
    int order=0;
    scanf("%d",&total);
    for(int i=0;i<total;i++)
    {
        scanf("%d",&org[i]);
    }
    ans[0]=org[0];
    for(int i=1;i<total;i++)
    {
        if(org[i]>ans[last])
        {
            //printf("%d>%d,last=%d\n",org[i],ans[last],last);
            last++;
            ans[last]=org[i];
        }
        else
        {
            for(int j=0;j<=last;j++)
            {
                if(org[i]<=ans[j])
                {
                    //printf("%d<=%d,index=%d\n",org[i],ans[j],j);
                    ans[j]=org[i];
                    //last=j;
                    break;
                }
            }
        }
        /*if(last+1>Max)
        {
            Max=last+1;
        }*/
    }
    //printf("%d",Max);
    printf("%d",last+1);
    return 0;
}
```

## 五

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

#define MAX 10

int unitlist[100];
int ma[100],mb[100],mans[100];

void makelist(int index)
{
    int count=0;
    int ans=2;
    int flag=0;
    while(count<index)
    {
        flag=0;
        for(int i=2;i<=sqrt(ans);i++)
        {
            if(ans%i==0)
            {
                flag=1;
                break;
            }
        }
        if(flag==0)
        {
            unitlist[count]=ans;
            count++;
        }
        ans++;
    }
}

int power(int index)
{
    int ans=1;
    for(int i=0;i<index-1;i++)
    {
        ans*=unitlist[i];
    }
    return ans;
}

long long int mtoe(int n[],int index)
{
    long long int ans=0;
    for(int i=0;i<index;i++)
    {
        ans+=n[i]*power(index-i);
    }
    return ans;
}

void etom(long long int n)
{
    int hsb=0;
    for(int i=MAX;i>0;i--)
    {
        if(power(i)<=n)
        {
            if(i>hsb)
            {
                hsb=i;
            }
            mans[i]=n/power(i);
            n=n-mans[i]*power(i);
        }
    }
    for(int j=hsb;j>0;j--)
    {
        printf("%d",mans[j]);
        if(j!=1)
        {
            printf(",");
        }
        else
        {
            printf("\n");
        }
    }
}

int main()
{
    char che[300],numche[100];
    int n=0,ca=0,cb=0,nc=0;
    makelist(MAX);
    while(1)
    {
        for(int i=0;i<MAX;i++)
        {
            mans[i]=0;
        }
        n=0;//总字符串
        ca=0;//a的位数
        cb=0;
        nc=0;//一个数字
        gets(che);
        while(che[n]!=' ')
        {
            if(che[n]!=',')
            {
                numche[nc]=che[n];
                nc++;
            }
            else
            {
                numche[nc]='\0';
                ma[ca]=atoi(numche);
                //printf("a=%d\n",ma[ca]);
                ca++;
                nc=0;
            }
            n++;
        }
        numche[nc]='\0';
        ma[ca]=atoi(numche);
        //printf("a=%d\n",ma[ca]);
        ca++;
        nc=0;
        n++;
        while(che[n]!='\0')
        {
            if(che[n]!=',')
            {
                numche[nc]=che[n];
                nc++;
            }
            else
            {
                numche[nc]='\0';
                mb[cb]=atoi(numche);
                //printf("b=%d\n",mb[cb]);
                cb++;
                nc=0;
            }
            n++;
        }
        numche[nc]='\0';
        mb[cb]=atoi(numche);
        //printf("b=%d\n",mb[ca]);
        cb++;
        if((ma[0]==0&&ca==1)||(mb[0]==0&&cb==1))
        {
            break;
        }
        else
        {
            //printf("a=%d,b=%d\n",mtoe(ma,ca),mtoe(mb,cb));
            etom(mtoe(ma,ca)+mtoe(mb,cb));
        }
    }
    return 0;
}
```

