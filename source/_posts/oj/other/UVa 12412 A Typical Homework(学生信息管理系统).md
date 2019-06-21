---
title: UVa 12412 A Typical Homework(学生信息管理系统)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2014-09-02 09:50:36
---
## A Typical Homework(a.k.a Shi Xiong Bang Bang Mang)

Hi, I am an undergraduate student in institute of foreign languages. As you know, C programming is a required course in our university, even if his/her major is far from computer science. I don't like this course at all, as I am not good at computer and I don't wanna even have a try of any programming!! But I have to do the homework in order to pass :( Sh... Could you help me with it? Please keep secret!! I know that you won't say NO to a poor little girl, boy. :)

## Task

Write a Student Performance Management System (SPMS).

## Concepts

In the SPMS, there will be at most 100 students, each has an SID, a CID, a name and four scores (Chinese, Mathematics, English and Programming).

## Main Menu

When you enter the SPMS, the main menu should be shown like this: Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit

## Adding a Student

If you choose 1 from the main menu, the following message should be printed on the screen: Please enter the SID, CID, name and four scores. Enter 0 to finish. Then your program should wait for user input. The input lines are always valid (no invalid SID, CID, or name, exactly four scores etc), but the SID may already exist. In that case, simply ignore this line and print the following: Duplicated SID. On the other hand, multiple students can have the same name. You should keep printing the message above until the user inputs a single zero. After that the main menu is printed again.

## Removing a Student

If you choose 2 from the main menu, the following message should be printed on the screen: Please enter SID or name. Enter 0 to finish. Then your program should wait for user input, and remove all the students matching the SID or name in the database, and print the following message (it's possible xx=0): xx student(s) removed. You should keep printing the message above until the user inputs a single zero. After that the main menu is printed again.

## Querying Students

If you choose 3 from the main menu, the following message should be printed on the screen: Please enter SID or name. Enter 0 to finish. Then your program should wait for user input. If no student matches the SID or name, simply do nothing, otherwise print out all the matching students, in the same order they're added to the database. The format is similar to the input format for "adding a student", but 3 more columns are added: rank (1st column), total score and average score (last two columns). The student with highest total score (considering all classes) received rank-1, and if there are two rank-2 students, the next one would be rank-4. You should keep printing the message above until the user inputs a single zero. After that the main menu is printed again.

## Showing the Ranklist

If you choose 4 from the main menu, the following message should be printed on the screen: Showing the ranklist hurts students' self-esteem. Don't do that. Then the main menu is printed again.

## Showing Statistics

If you choose 5 from the main menu, show the statistics, in the following format: Please enter class ID, 0 for the whole statistics. When a class ID is entered, print the following statistics. Note that "passed" means to have a score of at least 60. Chinese Average Score: xx.xx Number of passed students: xx Number of failed students: xx Mathematics Average Score: xx.xx Number of passed students: xx Number of failed students: xx English Average Score: xx.xx Number of passed students: xx Number of failed students: xx Programming Average Score: xx.xx Number of passed students: xx Number of failed students: xx Overall: Number of students who passed all subjects: xx Number of students who passed 3 or more subjects: xx Number of students who passed 2 or more subjects: xx Number of students who passed 1 or more subjects: xx Number of students who failed all subjects: xx Then, the main menu is printed again.

## Exiting SPMS

If you choose 0 from the main menu, the program should terminate. Note that course scores and total score should be formatted as integers, but average scores should be formatted as a real number with exactly two digits after the decimal point.

## Input

There will be a single test case, ending with a zero entered in the main menu screen. The entire input will be valid. The size of input does not exceed 10KB.

## Output

Print out everything as stated in the problem description. You should be able to play around this little program in your machine, with a keyboard and a screen. However, both the input and output may look silly when they're not mixed, as in the keyboard-screen case.

## Sample Input

1 0011223344 1 John 79 98 91 100 0022334455 1 Tom 59 72 60 81 0011223344 2 Alice 100 100 100 100 2423475629 2 John 60 80 30 99 0 3 0022334455 John 0 5 1 2 0011223344 0 5 0 4 0

## Output for the Sample Input

Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit Please enter the SID, CID, name and four scores. Enter 0 to finish. Please enter the SID, CID, name and four scores. Enter 0 to finish. Please enter the SID, CID, name and four scores. Enter 0 to finish. Duplicated SID. Please enter the SID, CID, name and four scores. Enter 0 to finish. Please enter the SID, CID, name and four scores. Enter 0 to finish. Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit Please enter SID or name. Enter 0 to finish. 2 0022334455 1 Tom 59 72 60 81 272 68.00 Please enter SID or name. Enter 0 to finish. 1 0011223344 1 John 79 98 91 100 368 92.00 3 2423475629 2 John 60 80 30 99 269 67.25 Please enter SID or name. Enter 0 to finish. Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit Please enter class ID, 0 for the whole statistics. Chinese Average Score: 69.00 Number of passed students: 1 Number of failed students: 1 Mathematics Average Score: 85.00 Number of passed students: 2 Number of failed students: 0 English Average Score: 75.50 Number of passed students: 2 Number of failed students: 0 Programming Average Score: 90.50 Number of passed students: 2 Number of failed students: 0 Overall: Number of students who passed all subjects: 1 Number of students who passed 3 or more subjects: 2 Number of students who passed 2 or more subjects: 2 Number of students who passed 1 or more subjects: 2 Number of students who failed all subjects: 0 Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit Please enter SID or name. Enter 0 to finish. 1 student(s) removed. Please enter SID or name. Enter 0 to finish. Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit Please enter class ID, 0 for the whole statistics. Chinese Average Score: 59.50 Number of passed students: 1 Number of failed students: 1 Mathematics Average Score: 76.00 Number of passed students: 2 Number of failed students: 0 English Average Score: 45.00 Number of passed students: 1 Number of failed students: 1 Programming Average Score: 90.00 Number of passed students: 2 Number of failed students: 0 Overall: Number of students who passed all subjects: 0 Number of students who passed 3 or more subjects: 2 Number of students who passed 2 or more subjects: 2 Number of students who passed 1 or more subjects: 2 Number of students who failed all subjects: 0 Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit Showing the ranklist hurts students' self-esteem. Don't do that. Welcome to Student Performance Management System (SPMS). 1 - Add 2 - Remove 3 - Query 4 - Show ranking 5 - Show Statistics 0 - Exit  Rujia Liu's Present 5: Developing Simplified Softwares
Special Thanks: Youzhi Bao, Zhuohua Chen

这种题目出现在oj上感觉挺奇怪的 这不是当年C语言期末考试的题目吗 实在不知道我的代码哪里有问题 样例是一摸一样的

可能哪里格式错了吧 既然是个系统就不在乎那些了TT

```js 
#include<cstdio>
#include<iostream>
#include<string>
#include<map>
using namespace std;
#define eps 1e-5
map<string, int> si;
int n;
struct Student
{
    string sid, name;
    int cid, chi, mat, eng, pro, sco;
    Student () {}
    Student (string s, int c, string na, int s1, int s2, int s3, int s4)
        : sid (s), cid (c), name (na), chi (s1), mat (s2), eng (s3), pro (s4)
    {
        sco = chi + mat + eng + pro;
    }
    void print()
    {
        cout << sid << " " << cid << " " << name;
        printf (" %d %d %d %d %d %.2lf\n", chi, mat, eng, pro, sco, sco / 4.0 + eps);
    }
} stu[100000];


void Add()
{
    string s, na;
    int c, s1, s2, s3, s4;
    printf ("Please enter the SID, CID, name and four scores. Enter 0 to finish.\n");
    while (cin >> s, s != "0")
    {
        cin >> c >> na >> s1 >> s2 >> s3 >> s4;
        if (si[s]) printf ("Duplicated SID.\n");
        else
        {
            si[s] = (++n);
            stu[n] = Student (s, c, na, s1, s2, s3, s4);
        }
        printf ("Please enter the SID, CID, name and four scores. Enter 0 to finish.\n");
    }
}


void Remove()
{
    string s;
    int t;
    printf ("Please enter SID or name. Enter 0 to finish.\n");
    while (cin >> s, s != "0")
    {
        t = 0;
        for (int i = 1; i <= n; ++i)
            if (si[stu[i].sid])
                if (stu[i].name == s || stu[i].sid == s)
                {
                    si[stu[i].sid] = 0;
                    ++t;
                }
        printf ("%d student(s) removed.\n", t);
        printf ("Please enter SID or name. Enter 0 to finish.\n");
    }
}

void Query()
{
    int t;
    string s;
    printf ("Please enter SID or name. Enter 0 to finish.\n");
    while (cin >> s, s != "0")
    {
        for (int i = t = 1; i <= n; ++i)
            if (si[stu[i].sid])
            {
                if (stu[i].sid == s || stu[i].name == s)
                {
                    printf ("%d ", t++);
                    stu[i].print();
                }
                else  ++t;
            }
        printf ("Please enter SID or name. Enter 0 to finish.\n");
    }
}


void Show_ranking()
{
    printf ("Showing the ranklist hurts students' self-esteem. Don't do that.\n");
}

void Show_Statistics()
{
    printf ("Please enter class ID, 0 for the whole statistics.\n");
    int schi = 0, seng = 0, smat = 0, spro = 0, pmat = 0, cla,
        pchi = 0, peng = 0, ppro = 0, npa = 0, np1 = 0, np2 = 0, np3 = 0, nfa = 0, numb;
    scanf ("%d", &cla);
    for (int i = 1; i <= n; ++i)
        if ( (stu[i].cid == cla || cla == 0) && si[stu[i].sid])
        {
            int t = 0;
            schi += stu[i].chi;
            if (stu[i].chi >= 60)
            {
                ++pchi;
                ++t;
            }
            seng += stu[i].eng;
            if (stu[i].eng >= 60)
            {
                ++peng;
                ++t;
            }
            spro += stu[i].pro;
            if (stu[i].pro >= 60)
            {
                ++ppro;
                ++t;
            }
            smat += stu[i].mat;
            if (stu[i].mat >= 60)
            {
                ++pmat;
                ++t;
            }
            if (t == 0) ++nfa;
            if (t == 1) ++np1;
            if (t == 2) ++np2;
            if (t == 3) ++np3;
            if (t == 4) ++npa;
        }
    numb = nfa + np1 + np2 + np3 + npa;
    printf ("Chinese\n");
    printf ("Average Score: %.2lf\n", schi / (numb * 1.0) + eps);
    printf ("Number of passed students: %d\n", pchi);
    printf ("Number of failed students: %d\n\n", numb - pchi);
    
    printf ("Mathematics\n");
    printf ("Average Score: %.2lf\n", smat / (numb * 1.0) + eps);
    printf ("Number of passed students: %d\n", pmat);
    printf ("Number of failed students: %d\n\n", numb - pmat);
    
    printf ("English\n");
    printf ("Average Score: %.2lf\n", seng / (numb * 1.0) + eps);
    printf ("Number of passed students: %d\n", peng);
    printf ("Number of failed students: %d\n\n", numb - peng);
    
    printf ("Programming\n");
    printf ("Average Score: %.2lf\n", spro / (numb * 1.0) + eps);
    printf ("Number of passed students: %d\n", ppro);
    printf ("Number of failed students: %d\n\n", numb - ppro);
    
    printf ("Overall:\n");
    printf ("Number of students who passed all subjects: %d\n", npa);
    printf ("Number of students who passed 3 or more subjects: %d\n", npa + np3);
    printf ("Number of students who passed 2 or more subjects: %d\n", npa + np3 + np2);
    printf ("Number of students who passed 1 or more subjects: %d\n", npa + np3 + np2 + np1);
    printf ("Number of students who failed all subjects: %d\n\n", nfa);
}


int main()
{
    int cho;
    n = 0;
    while (1)
    {
        printf ("Welcome to Student Performance Management System (SPMS).\n\n");
        printf ("1 - Add\n");
        printf ("2 - Remove\n");
        printf ("3 - Query\n");
        printf ("4 - Show ranking\n");
        printf ("5 - Show Statistics\n");
        printf ("0 - Exit\n\n");
        scanf ("%d", &cho);
        if (cho == 1) Add();
        if (cho == 2) Remove();
        if (cho == 3) Query();
        if (cho == 4) Show_ranking();
        if (cho == 5) Show_Statistics();
        if (cho == 0)  return 0;
    }
}
```

刘汝佳的代码

```js 
// UVa12412 A Typical Homework (a.k.a Shi Xiong Bang Bang Mang)
// Rujia Liu
#include<stdio.h>
#include<string.h>
#define maxn 1000
#define maxl 100
#define EPS 1e-5

int n = 0;
char sid[maxn][maxl];
int cid[maxn];
char name[maxn][maxl];
int score[maxn][5];
int removed[maxn];

const char* course_name[] = {"Chinese", "Mathematics", "English", "Programming"};

int valid (int k)
{
    for (int i = 0; i < k; i++) if (!removed[i])
        {
            if (strcmp (sid[i], sid[k]) == 0) return 0;
        }
    return 1;
}

void add()
{
    for (;;)
    {
        printf ("Please enter the SID, CID, name and four scores. Enter 0 to finish.\n");
        scanf ("%s", sid[n]);
        if (strcmp (sid[n], "0") == 0) break;
        scanf ("%d%s%d%d%d%d", &cid[n], name[n], &score[n][0], &score[n][1], &score[n][2], &score[n][3]);
        if (valid (n))
        {
            score[n][4] = score[n][0] + score[n][1] + score[n][2] + score[n][3];
            n++;
        }
        else printf ("Duplicated SID.\n");
    }
}

int rank (int k)
{
    int r = 0;
    for (int i = 0; i < n; i++)
        if (!removed[i] && score[i][4] > score[k][4]) r++;
    return r + 1;
}

void DQ (int isq)
{
    char s[maxl];
    for (;;)
    {
        printf ("Please enter SID or name. Enter 0 to finish.\n");
        scanf ("%s", s);
        if (strcmp (s, "0") == 0) break;
        int r = 0;
        for (int i = 0; i < n; i++) if (!removed[i])
            {
                if (strcmp (sid[i], s) == 0 || strcmp (name[i], s) == 0)
                {
                    if (isq) printf ("%d %s %d %s %d %d %d %d %d %.2f\n", rank (i), sid[i], cid[i], name[i], score[i][0], score[i][1], score[i][2], score[i][3], score[i][4], score[i][4] / 4.0 + EPS);
                    else
                    {
                        removed[i] = 1;
                        r++;
                    }
                }
            }
        if (!isq) printf ("%d student(s) removed.\n", r);
    }
}

double get_course_stat (int c, int s, int* passed, int* failed)
{
    int tot = 0;
    *passed = *failed = 0;
    for (int i = 0; i < n; i++)
        if (!removed[i] && (c == 0 || cid[i] == c))
        {
            tot += score[i][s];
            if (score[i][s] >= 60) (*passed)++;
            else (*failed)++;
        }
    return (double) tot / (double) (*passed + *failed);
}

void get_overall_stat (int c, int* cnt)
{
    cnt[0] = cnt[1] = cnt[2] = cnt[3] = cnt[4] = 0;
    for (int i = 0; i < n; i++)
        if (!removed[i] && (c == 0 || cid[i] == c))
        {
            int k = 0;
            for (int j = 0; j < 4; j++) if (score[i][j] >= 60) k++;
            cnt[k]++;
        }
}

void stat()
{
    int c;
    printf ("Please enter class ID, 0 for the whole statistics.\n");
    scanf ("%d", &c);
    for (int i = 0; i < 4; i++)
    {
        int passed, failed;
        double avg = get_course_stat (c, i, &passed, &failed);
        printf ("%s\n", course_name[i]);
        printf ("Average Score: %.2f\n", avg + EPS);
        printf ("Number of passed students: %d\n", passed);
        printf ("Number of failed students: %d\n", failed);
        printf ("\n");
    }
    int cnt[5];
    get_overall_stat (c, cnt);
    printf ("Overall:\n");
    printf ("Number of students who passed all subjects: %d\n", cnt[4]);
    printf ("Number of students who passed 3 or more subjects: %d\n", cnt[4] + cnt[3]);
    printf ("Number of students who passed 2 or more subjects: %d\n", cnt[4] + cnt[3] + cnt[2]);
    printf ("Number of students who passed 1 or more subjects: %d\n", cnt[4] + cnt[3] + cnt[2] + cnt[1]);
    printf ("Number of students who failed all subjects: %d\n", cnt[0]);
    printf ("\n");
}

int main()
{
    for (;;)
    {
        int choice;
        printf ("Welcome to Student Performance Management System (SPMS).\n");
        printf ("\n");
        printf ("1 - Add\n");
        printf ("2 - Remove\n");
        printf ("3 - Query\n");
        printf ("4 - Show ranking\n");
        printf ("5 - Show Statistics\n");
        printf ("0 - Exit\n");
        printf ("\n");
        scanf ("%d", &choice);
        if (choice == 0) break;
        if (choice == 1) add();
        if (choice == 2) DQ (0);
        if (choice == 3) DQ (1);
        if (choice == 4) printf ("Showing the ranklist hurts students' self-esteem. Don't do that.\n");
        if (choice == 5) stat();
    }
    return 0;
}
```