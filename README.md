#include<iostream>
#include<cstring>
#include<sys/types.h>
#include<sys/stat.h>
#include<dirent.h>
#include<stdlib.h>
#include<fcntl.h>
#include<time.h>
#include<queue>
#include<ftw.h>
using namespace std;

char param_list[20][250];

//declare method
void mydir();
void mycd();
void mycopy();
void mydel();
void mypwd();
/**
* main method
* return 0
**/
int main(int argc, char *argv[])
{
   cout<<">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"<<endl;
   cout<<">>>>>>Welcome to use linux command!<<<<<<"<<endl;
   cout<<"You can use the commands as follows:"<<endl;
   cout<<"1. mydir "<<endl;
   cout<<"2. mycd"<<endl;
   cout<<"3. mycopy"<<endl;
   cout<<"4. mydel"<<endl;
   cout<<"5. mypwd"<<endl;
   cout<<">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"<<endl;

   string str;
   //when the str is  equal with the string "exit",break the cycling
   while(str != "exit") {
      //shell prompt
      cout<<"[linux@]$";
      cin>>str;      //enter the command
      if(str == "mydir"){
         // if str is equal with the string "pwd", execute the pwd() method
         mydir();
      }
      if(str == "mycd") {
        // if str is equal with the string "dir", execute the dir() method;
        mycd();
      }
      if(str == "mycopy") {
        // if str is equal with the string "dir", execute the cd() method;
         mycopy();
      }
      if(str == "mydel"){
       // if str is equal with the string "newdir", execute the newdir() meth
         mydel();
      }
      if(str == "mypwd"){
        // if str is equal with the string "deldir", execute the deldir() mte
         mypwd();
      }
    }
    return 0;

}
/**
* fuction:show current directory path
* return : void
**/
void mypwd()
{
   char ptr[80];   // create a character array with the size of 80
   getcwd(ptr,sizeof(ptr));   //invoke the getcwd() method
   cout<<ptr<<endl;  //print the directory path
}
/**
* fuction: show current directory's directory or file
* return : void
*/
void mydir()
{
  DIR * dir;   //The DIR data type represents a directory stream
  struct dirent* ptr;
  int count = 0;
  char *dirname;
  cin>>dirname;
  dir = opendir(dirname);
  if(dir == NULL)
  {
     cout<<"cannot open directory"<<endl;
  }
  //The opendir function opens and returns a directory stream for reading the
//rectory whose file name is dirname
  //This readdir function reads the next entry from the directory.
  while((ptr = readdir(dir)) != NULL)
  {    // if the d_name is equal with "." or ".." ,do nothing
      if(strcmp(ptr->d_name, ".") == 0 || strcmp(ptr->d_name, "..") == 0){}
      // if not , print the ptr->d_name
      else
          cout<<ptr->d_name<<" ";
     // count the d_name
      count++;
     // if count % 8 == 0, line feed
      if(count % 8 == 0)
         cout<<endl;

   }
   // close directory stream.
   closedir(dir);
   cout<<endl;
}
/**
* function; change the directory path
* return: void
*/
void mycd()
{
   char dirname[20];
   cin>>dirname;
   //if change the directory successful
   if(chdir(dirname) == -1)
   {
      cout<<"the directory is not exit!!!"<<endl;

    }
    else
    {
      cout<<"change directory success!!!"<<endl;
     }
}
/*  copy */
void mycopy(){
   char spath[250];
   char apath[250];
   FILE *newfp;
   FILE *oldfp;
   char ch;
   getcwd(spath,250);
   getcwd(apath,250);
   strcat(spath,"/");
   strcat(spath,param_list[1]);
   strcat(apath,"/");
   strcat(apath,param_list[2]);
   if((oldfp=fopen(spath,"r"))==NULL){
       cout<<"can't open the file!"<<endl;
   }
   if((newfp=fopen(apath,"w"))==NULL){
       cout<<"faile to built a file!"<<endl;
   }
   while((ch=fgetc(oldfp))!=EOF){
       fputc(ch,newfp);
   }
   fclose(oldfp);
   fclose(newfp);
   cout<<"success to copy the file!"<<endl;
}
/*  del   */
void mydel()


{
   char filename[20];
   cin >> filename;
   // if delete the dirtectory successful return 0
   if(rmdir(filename) == 0)
   {
    cout<<filename<<" delete successful!!!"<<endl;
   }
   else
       cout<<filename<<" delete failure!!!"<<endl;
}
