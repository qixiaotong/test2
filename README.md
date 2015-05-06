#include "stdio.h"
#include "stdlib.h"
#include "string.h"
#include "conio.h"   
#define LEN sizeof(struct LinkPerson)
#define FORMAT printf("\n姓名 手机  办公电话  家庭电话  电子邮箱  所在省市  工作单位  家庭住址  群组\n");
void print(struct LinkPerson *head);
int nn=0; //统计联系人的个数
char selete[10];
//姓名、手机、办公电话、家庭电话、电子邮箱、所在省市、工作单位、家庭住址，群组分类(亲属、同事、同学、朋友、其他)
typedef struct LinkPerson
{
 char name[20], mobile[15], office_ph[15], home_ph[15], E_mail[40], in_cities[20], work_units[40],address[40],group[20];
 struct LinkPerson *next;
}LP;
LP *creat()  //创建
{
 LP *head,*p1,*p2;
 char ch[2]={"1"};
 printf("\n请输入通讯录联系人信息：\n\n");
 FORMAT;
 p1=p2=(struct LinkPerson *)malloc(LEN);
 scanf("%s%s%s%s%s%s%s%s%s",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
 while( strcmp(p1->name,"0")!=0 )
 {
  nn++;
  if(nn==1) head=p1;
  else p2->next=p1;
  p2=p1;
  p1=(struct LinkPerson *)malloc(LEN);
scanf("%s%s%s%s%s%s%s%s%s",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
 }
 p2->next=NULL;
 free(p1);
 return head;
}
void save(LP *head) //函数功能：保存文件
{
 FILE *out;
 LP *p1=head;
 putchar(10);
 if ((out=fopen("people.txt", "w+"))==NULL)
        { printf("Can not open this file!\n");  exit(0); }
 fclose(out);
if ((out=fopen("people.txt", "r+"))==NULL)
        { printf("Can not open this file!\n");  exit(0); }
 for(int i=0; i<nn; i++)
 {
fprintf(out,"%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\n",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
  p1=p1->next;
 } 
 fclose(out); 
 putchar(10);
}
LP *read(void)  //函数功能：读取文件
{
 FILE *fp;
 LP *head=NULL, *p1=NULL, *p2=NULL;
 int m=0;
 
 if ((fp=fopen("people.txt", "r"))==NULL)
        { printf("Can not open this file!\n");  exit(0); } 
 while(!feof(fp))
 {  
  p1=(LP *)malloc(LEN); 
fscanf(fp,"%s%s%s%s%s%s%s%s%s\n",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
  m+=1;
  if(m == 1) head=p1;
  else p2->next=p1;
  p2=p1;
  }
 p2->next=NULL;
 fclose(fp);
 nn=m;
 return(head);
}
LP *array(LP *head)  //函数功能：排序
{
 LP *p0,*p1,*p2,*h;
 h=p1=p2=head;
 if(nn<=1) return(h);
 for(int i=0; i<nn-1; i++)
 {
  p1=p2=h;
  for(int j=0; j<nn-i-1; j++)
  {
    p2=p1->next;  
   if((p1==h) && strcmp(p1->name,p2->name)>0 ) 
   {  
    h=p2; 
    p1->next=p2->next; 
    p2->next=p1; 
    p0=p2;   
   }
   else if( strcmp(p1->name,p2->name)>0 ) 
    { 
     p0->next=p2; 
     p1->next=p2->next; 
     p2->next=p1;
     p0=p2;
    }
   else { p0=p1; p1=p2; p2=p2->next;  }
  }
 }
 return(h);
}
void print()  //函数功能 ：输出信息
{
  LP *p1,*head;
 p1=head=read();
  printf("共有联系人 %d 名\n\n",nn);
 FORMAT;
 if(head!=NULL)
  do
  {
   printf("%s %s %s %s %s %s %s %s %s\n",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
   p1=p1->next;
  }while(p1!=NULL);
}
LP *add(void) //函数功能：增加联系人信息
{
 LP *head,*p1,*p2;
 head=read();
 p2=head;
while(p2->next != NULL) //找到原先数据的终点，作为新增数据的起点
 {
  p2=p2->next;
 }
 p1=(LP *)malloc(LEN);
 printf("请输入增加联系人的信息：\n");
 FORMAT;
scanf("%s%s%s%s%s%s%s%s%s",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
 while( strcmp(p1->name,"0")!=0 )
 {
  nn++;
  if(nn==1) head=p1;
  else p2->next=p1;
  p2=p1;
  p1=(struct LinkPerson *)malloc(LEN);
  scanf("%s%s%s%s%s%s%s%s%s",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
  //
 }
 p2->next=NULL;
 free(p1);
  head=array(head);
 save(head); 
 return (head);
}
LP *del()  //函数功能：删除信息
{
start_del:
 char name[20];
 LP *head,*p1,*p2;
 p1=p2=head=read();
 printf("请输入要删除的联系人姓名: ");
 scanf("%s",name);
 while( strcmp(p1->name,name)!=0 && (p1->next!=NULL) )     
  { p2=p1; p1=p1->next; } //找出p1指向的节点
 if( strcmp(p1->name,name)==0)
 {
  if(p1==head) head=p1->next; 
  else p2->next=p1->next;
  printf("del: %s\n",name);
  free(p1);
  nn--; 
  printf("还有联系人%d位\n",nn);
 }
 else printf("没有你要删除的联系人！\n");
 save(head); 
 printf("\n是否继续进行删除操作? 1.是 2.返回主菜单 3.退出 \n请输入：");
 scanf("%s",selete);
 if( strcmp(selete,"1")==0 ) goto start_del;
 else if( strcmp(selete,"2")==0 ) return 0;
 else if( strcmp(selete,"3")==0 ) exit(0);
 else return(head);
 }
LP *modify() //函数功能：修改联系人信息
{
start_mod:
 char name[20];
 int select;
 LP *head,*p1,*p2;
 p1=p2=head=read();
 printf("请输入要修改的联系人姓名: ");
 scanf("%s",name);
 while( strcmp(p1->name,name)!=0 && (p1->next!=NULL) )     
  { p2=p1; p1=p1->next; } //找出p1指向的节点
 if( strcmp(p1->name,name)==0 )
 {
  printf("将要修改的联系人的信息\n");
  FORMAT;
  printf("%s %s %s %s %s %s %s %s %s\n",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
  printf("请输入要修改的选项\n");
  printf(" 1.姓名\t    2.手机\t3.办公电话\t4.家庭电话  5.电子邮箱\n 6.所在省市 7.工作单位  8.家庭住址\t9.群组\n请输入：");
  scanf("%d",&select);
  printf("请输入该项新的信息：");
  if(select==1) { scanf("%s", p1->name); }
  if(select==2) { scanf("%s",p1->mobile); }
  if(select==3) { scanf("%s",p1->office_ph); }
  if(select==4) { scanf("%s",p1->home_ph); }
  if(select==5) { scanf("%s",p1->E_mail); }
  if(select==6) { scanf("%s",p1->in_cities); }
  if(select==7) { scanf("%s",p1->work_units); }
  if(select==8) { scanf("%s",p1->address); }
  if(select==9) { scanf("%s",p1->group); }
  
  save(head);
 }
 else { printf("\n没有该联系人，请重新输入！\n\n"); goto start_mod; }

 printf("\n是否继续进行修改操作? 1.是 2.返回主菜单 3.退出 \n请输入：");
 scanf("%s",selete);

 if( strcmp(selete,"1")==0 ) goto start_mod;
 else if( strcmp(selete,"2")==0 ) return 0;
 else if( strcmp(selete,"3")==0 ) exit(0);
 else return(head);
}
 
find() //函数功能 ：查找
{
  char select[10];
 char name[20],mobile[15],group[20];
 LP *p1,*p2,*head;
start_f:
 p1=p2=head=read();
 printf("按下列选项查询联系人信息\n");
 printf(" 1.按姓名  2.按手机号码  3.按群组分类  \n 请选择：");
 scanf("%s",select);

 if( strcmp(select,"1")==0 )
 {
  printf("请输入姓名：");
  scanf("%s",name);
  while( strcmp(p1->name,name)!=0 && (p1->next!=NULL) )     
  { p2=p1; p1=p1->next; } //找出p1指向的节点
  if( strcmp(p1->name,name)==0 )
  {
   FORMAT;
   printf("%s %s %s %s %s %s %s %s %s\n",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
  }
  else printf("没有要查找的联系人信息！！\n");
 }
 else if( strcmp(select,"2")==0 )
 { 
  printf("请输入手机号码：");
  scanf("%s",mobile);
  while( strcmp(p1->mobile,mobile)!=0 && (p1->next!=NULL) )     
   { p2=p1; p1=p1->next; } //找出p1指向的节点
  if(strcmp(p1->mobile,mobile)==0)
  {
   FORMAT;
   printf("%s %s %s %s %s %s %s %s %s\n",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
  }
  else printf("没有要查找的联系人信息！！\n"); 
 }
 else if( strcmp(select,"3")==0 )
 { 
  int n_n=0;
  printf("请输入群组:");
  scanf("%s",group);
  while( strcmp(p1->group,group)!=0 && (p1->next!=NULL) )     
   { p2=p1; p1=p1->next; } //找出p1指向的节点
  if(strcmp(p1->group,group)==0)
  {    
   FORMAT;
   printf("%s %s %s %s %s %s %s %s %s\n",p1->name,p1->mobile,p1->office_ph,p1->home_ph,p1->E_mail,p1->in_cities,p1->work_units,p1->address,p1->group);
  }
  else printf("没有要查找的联系人信息！！\n");
 }
 else { printf("\n请输入正确的选择！！\n"); goto start_f; }
 printf("\n 1.继续查找  2.返回主菜单  3.退出 \n请选择：");
 scanf("%s",select);
 if( strcmp(select,"1")==0 ) goto start_f;
 else if( strcmp(select,"2")==0 ) return 0;
 else if( strcmp(select,"3")==0 ) exit(0);
 else return 0;
}
void main()
{
 printf("\n \2\1\t\t\t 欢迎使用通讯录管理系统 \n\n");
start:
 LP *head;
 printf("\n 1.新建联系人 2.添加联系人 3.删除 4.修改 5.查询 6.输出联系人的信息 7.退出 \n请输入选择：");
 scanf("%s",selete);
 if( strcmp(selete,"1")==0 ) { head=creat(); head=array(head); goto start; }
 else if( strcmp(selete,"2")==0 ) { head=add(); goto start; } 
 else if( strcmp(selete,"3")==0 ) { head=del();  goto start;}
 else if( strcmp(selete,"4")==0 ) { modify();  goto start;}
 else if( strcmp(selete,"5")==0 ) { find();  goto start;} 
 else if( strcmp(selete,"6")==0 ) { print();  goto start;} 
 else if( strcmp(selete,"7")==0 ) exit(0);
 else goto start;
//
}
