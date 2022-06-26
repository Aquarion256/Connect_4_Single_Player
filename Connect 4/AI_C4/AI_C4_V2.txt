#include<stdio.h>
#include<conio.h>
#include<stdlib.h>
#include<time.h>
time_t tim=time(NULL);
char board[100][100];
int i,j,h=6,w=7,r,c;
char pce,p1='X',p2='O';
int k[100];
int count[4],t1,t2;
void Start()
{
    for(i=0;i<w;i++)
    {
        k[i]=5;
    }
    for(int i=0;i<h;i++)
    {
        for(int j=0;j<w;j++)
        {
            board[i][j]='_';
        }
    }
}
void Board()
{
    for(int i=0;i<h;i++)
    {
        for(int j=0;j<w;j++)
        {
            printf("%c ",board[i][j]);
        }
        printf("\n");
    }
    for(int i=1;i<=w;i++)
    {
        printf("%d ",i);
    }
    printf("\n");
}
int wcheck(int x,int y,char tok,int choice)
{
    int maxcount,maxpos;
    int max_j[4],max_i[4],min_i[4],min_j[4];
    for(i=0;i<4;i++)
    {
        count[i]=0;
        max_i[i]=x;
        max_j[i]=y;
        min_i[i]=x;
        min_j[i]=y;
    }
    //horizontal
    t2=y+1;count[0]=0;
    while(t2<w && board[x][t2]==tok)
    {
        count[0]++;
        max_j[0]=t2;
        t2++;
    }
    t2=y-1;
    while(t2>=0 && board[x][t2]==tok)
    {
        count[0]++;
        min_j[0]=t2;
        t2--;
    }
    //vertical
    t1=x+1;count[1]=0;
    while(t1<h && board[t1][y]==tok)
    {
        count[1]++;
        min_i[1]=t1;
        t1++;
    }
    t1=x-1;
    while(t1>=0 && board[t1][y]==tok)
    {
        count[1]++;
        max_i[1]=t1;
        t1--;
    }
    //first diagonal
    t1=x+1;t2=y+1;count[2]=0;
    while(t1<h && t2<w && board[t1][t2]==tok)
    {
        count[2]++;
        max_i[2]=t1;max_j[2]=t2;
        t1++;t2++;
    }
    t1=x-1;t2=y-1;
    while(t1>=0 && t2>=0 && board[t1][t2]==tok)
    {
        count[2]++;
        min_i[2]=t1;min_j[2]=t2;
        t1--;t2--;
    }
    //second diagonal
    t1=x-1;t2=y+1;count[3]=0;
    while(t1>=0 && t2<w  && board[t1][t2]==tok)
    {
        count[3]++;
        max_i[3]=t1;max_j[3]=t2;
        t1--;t2++;
    }
    t1=x+1;t2=y-1;
    while(t1<h && t2>=0 && board[t1][t2]==tok)
    {
        count[3]++;
        min_i[3]=t1;min_j[3]=t2;
        t1++;t2--;
    }
    maxcount=count[0];
    maxpos=0;
    for(i=0;i<4;i++)
    {
        if(count[i]>maxcount)
        {
            maxcount=count[i];
            maxpos=i;
        }
    }
    if(choice==1)
        return maxcount;
    else if(choice==2)
        return max_i[maxpos];
    else if(choice==3)
        return max_j[maxpos];
    else if(choice==4)
        return min_i[maxpos];
    else if(choice==5)
        return min_j[maxpos];
    else
        return maxpos;
}
int comp_rand()
{
    srand(time(0));
    int rnd;
    rnd=rand()%10;
    return rnd;
}
int block(int MI,int MJ,int LI,int LJ,int MP,char FTOK)
{
    int play;
    //printf("MP=%d\n",MP);
    switch(MP)
    {
        case 0:
        //min check
        if(board[LI][LJ-1]=='_')
        {
            if(LI==h-1)
            {
                play=LJ;
                //printf("blocked\n");
            }
            else if(board[LI+1][LJ-1]!='_')
            {
                play=LJ;
                //printf("blocked\n");
            }
        }
        //max check
        else if(board[MI][MJ+1]=='_')
        {
            if(MI==h-1)
            {
                play=MJ+2;
                //printf("blocked\n");
            }
            else if(board[MI+1][MJ+1]!='_')
            {
                play=MJ+2;
                //printf("blocked\n");
            }
        }
        else
            play=100;
        break;
        case 1:
        //only check
        if(board[MI-1][MJ]=='_')
        {
            play=MJ+1;
            //printf("blocked\n");
        }
        else
        {
            play=100;
        }
        break;
        case 2:
        //printf("ENTERED\n");
        if(board[MI+1][MJ+1]=='_')
        {
            //printf("ENTERED2\n");
            if(MI+1==h-1)
            {
                //printf("blocked\n");
                play=MJ+2;
            }
            else if(board[MI+2][MJ+1]!='_')
            {
                //printf("blocked\n");
                play=MJ+2;
            }
        }//need to check is this works or not.
        else if(board[LI-1][LJ-1]=='_')
        {
            //printf("ENTERED3\n");
            if(board[LI][LJ-1]!='_')
            {
                //printf("blocked\n");
                play=LJ;
            }
        }
        else
            play=100;
        break;
        case 3:
        if(board[MI-1][MJ+1]=='_')
        {
            //printf("ENTERED4\n");
            if(board[MI][MJ+1]!='_')
            {
                //printf("blocked\n");
                play=MJ+2;
            }
        }
        else if(board[LI+1][LJ-1]=='_')
        {
            //printf("ENTERED5\n");
            if(LI+1==h-1)
            {
                //printf("blocked\n");
                play=LJ;
            }
            else if(board[LI+2][LJ-1]!='_')
            {
                //printf("blocked\n");
                play=LJ;
            }
        }
        else  
            play=100;
        break;
        default:
        play=100;
        break;
    }
    return play;
}
int gap(int gi,int gj,char gp)
{
        //printf("ENTERED GAP\n");
        if(board[gi][gj-2]==gp && board[gi][gj-1]=='_')
        {
            //printf("FOUND GAP\n");
            //printf("blocked\n");
            return gj;
        }
        else if(board[gi][gj+2]==gp && board[gi][gj+1]=='_')
        {
            //printf("FOUND GAP2\n");
            //printf("blocked\n");
            return gj+2;
        }
        else if(board[gi-2][gj-2]==gp && board[gi-1][gj-1]=='_')
        {
            //printf("FOUND GAP3\n");
            //printf("blocked\n");
            return gj;
        }
        else if(board[gi+2][gj+2]==gp && board[gi+1][gj+1]=='_')
        {
            //printf("FOUND GAP4\n");
            //printf("blocked\n");
            return gj+2;
        }
        else if(board[gi-2][gj+2]==gp && board[gi-1][gj+1]=='_')
        {
            //printf("FOUND GAP5\n");
            //printf("blocked\n");
            return gj+2;
        }
        else if(board[gi+2][gj-2]==gp && board[gi+1][gj-1]=='_')
        {
            //printf("FOUND GAP6\n");
            //printf("blocked\n");
            return gj;
        }
        else 
            return 100;
}
int not_comp_play(int pi,int pj,int ci,int cj)
{
    int pcount,ccount,fcount;
    int mi,mj,li,lj,mp;
    char ftok;int fi,fj;
    pcount=wcheck(pi,pj,p1,1);
    ccount=wcheck(ci,cj,p2,1);
    if(pcount>ccount)
    {
        fcount=pcount;
        ftok=p1;
        fi=pi;fj=pj;
    }
    else
    {
        fcount=ccount;
        ftok=p2;
        fi=ci;fj=cj;
    }
    // fi=pi;fj=pj;fcount=pcount;ftok=p1;
    mi=wcheck(fi,fj,ftok,2);
    mj=wcheck(fi,fj,ftok,3);
    li=wcheck(fi,fj,ftok,4);
    lj=wcheck(fi,fj,ftok,5);
    mp=wcheck(fi,fj,ftok,6);
    if(fcount==2 && block(mi,mj,li,lj,mp,ftok)!=100)
    {
        return block(mi,mj,li,lj,mp,ftok);
    }
    else  if(fcount==1 && block(mi,mj,li,lj,mp,ftok)!=100)
    {
        return block(mi,mj,li,lj,mp,ftok);
    }
    else
        return comp_rand();
    
}
int comp_play(int pi,int pj,int ci,int cj)
{
    int pcount,ccount;
    int mi,mj,li,lj,mp;
    pcount=wcheck(pi,pj,p1,1);
    ccount=wcheck(ci,cj,p2,1);
    if(ccount==2)
    {
        mi=wcheck(ci,cj,p2,2);
        mj=wcheck(ci,cj,p2,3);
        li=wcheck(ci,cj,p2,4);
        lj=wcheck(ci,cj,p2,5);
        mp=wcheck(ci,cj,p2,6);
        if(block(mi,mj,li,lj,mp,p2)!=100)
        {
            return block(mi,mj,li,lj,mp,p2);
        }
    }
    if(pcount==2)
    {
        mi=wcheck(pi,pj,p1,2);
        mj=wcheck(pi,pj,p1,3);
        li=wcheck(pi,pj,p1,4);
        lj=wcheck(pi,pj,p1,5);
        mp=wcheck(pi,pj,p1,6);
        if(block(mi,mj,li,lj,mp,p1)!=100)
        {
            return block(mi,mj,li,lj,mp,p1);
        }
    }
    if(gap(pi,pj,p1)!=100)
    {
        return gap(pi,pj,p1);
    }
    if(pcount==1)
    {
        mi=wcheck(pi,pj,p1,2);
        mj=wcheck(pi,pj,p1,3);
        li=wcheck(pi,pj,p1,4);
        lj=wcheck(pi,pj,p1,5);
        mp=wcheck(pi,pj,p1,6);
        if(block(mi,mj,li,lj,mp,p1)!=100)
        {
            return block(mi,mj,li,lj,mp,p1);
        }
    }
    if(ccount==1)
    {
        mi=wcheck(ci,cj,p2,2);
        mj=wcheck(ci,cj,p2,3);
        li=wcheck(ci,cj,p2,4);
        lj=wcheck(ci,cj,p2,5);
        mp=wcheck(ci,cj,p2,6);
        if(block(mi,mj,li,lj,mp,p2)!=100)
        {
            return block(mi,mj,li,lj,mp,p2);
        }
    }
    return comp_rand();
}
void S_Input()
{
    int g_o=0,turns=0,one,two,row,r,c,cchoice=0,PI,PJ,CI=0,CJ=0;
    while(g_o==0)
    {
        if(turns%2==0)
        {
            
            printf("Player 1's Turn.\n");
            if(cchoice)
                printf("Last move played was in column %d\n",cchoice);
            Board();
            scanf("%d",&one);
            if(one>0 && one<=w && k[one-1]>=0)
            {
                row=k[one-1];
                board[row][one-1]=p1;
                k[one-1]--;
                r=row;c=one-1;
                PI=row;PJ=c;
                turns++;
                pce=p1;
            }
            else
            {
                printf("Invalid input try again.\n");
                continue;
            }
        }
        else
        {
            cchoice=comp_play(PI,PJ,CI,CJ);
            //printf("CCHOICE=%d\n",cchoice);
            if(cchoice<=0)
            {
                while(cchoice!=1)
                {
                    cchoice++;
                }
            }
            else if(cchoice>7)
            {
                cchoice=cchoice-2;
            }
            else if(k[cchoice-1]<0)
            {
                cchoice=comp_rand();
            }
            if(cchoice>0 && cchoice<=w && k[cchoice-1]>=0)
            {
                row=k[cchoice-1];
                board[row][cchoice-1]=p2;
                k[cchoice-1]--;
                r=row;c=cchoice-1;
                CI=r;CJ=c;
                turns++;
                pce=p2;
            }
            else
            {
                continue;
            }
        }
        if(wcheck(r,c,pce,1)>=3)
        {
            g_o=1;
            Board();
        }
    }
}
void Input()
{
    int g_o=0,turns=0,one,two,row;
    while(g_o==0)
    {
        if(turns%2==0)
        {
            printf("Player 1's Turn.\n");
            Board();
            scanf("%d",&one);
            if(one>0 && one<=w && k[one-1]>=0)
            {
                row=k[one-1];
                board[row][one-1]=p1;
                k[one-1]--;
                r=row;c=one-1;
                turns++;
                pce=p1;
            }
            else
            {
                printf("Invalid input try again.\n");
                continue;
            }
        }
        else
        {
            printf("Player 2's Turn.\n");
            Board();
            scanf("%d",&two);
            if(two>0 && two<=w && k[two-1]>=0)
            {
                row=k[two-1];
                board[row][two-1]=p2;
                k[two-1]--;
                r=row;c=two-1;
                turns++;
                pce=p2;
            }
            else
            {
                printf("Invlaid input try again.\n");
                continue;
            }
        }
        if(wcheck(r,c,pce,1)>=3)
        {
            g_o=1;
            Board();
        }
    }
}
int main()
{
    while(true)
    {
        int y_n=0,choice;
        printf("Welcome to Connect 4!\n");
        Start();
        printf("Enter 1 for Single Player and 2 for Multiplayer.\n");
        scanf("%d",&choice);
        switch(choice)
        {
            case 2:
            Input();
            if(pce=='X')
            {
                printf("Player 1 Wins!\n");
            }
            else if(pce=='O')
            {
                printf("Player 2 Wins!\n");
            }
            else
            {   
                printf("It's a Draw you both suck!\n");
            }
            break;
            case 1:
            S_Input();
            if(pce=='X')
            {
                printf("Player 1 Wins!\n");
            }
            else if(pce=='O')
            {
                printf("Player 2 Wins!\n");
            }
            else
            {   
                printf("It's a Draw you both suck!\n");
            }
            break;
            default:
            printf("Invalid input please try again!\n");
            
        }
        printf("Would you like to play again? 1 or 0\n");
        scanf("%d",&y_n);
        if(y_n!=1)
        {
            printf("Goodbye!\n");
            break;
        }
        else{
            printf("\nPlaying again!\n");
        }
    }
    getch();
    return 0;
}