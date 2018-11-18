#include<iostream>
#include<fstream>
#include<stdlib.h>
#include<conio.h>
#include<ctype.h>
#include<windows.h>
#include<string.h>
#include<stdio.h>
#include<stdexcept>

using namespace std;

COORD coord= {0,0}; // this is global variable
void gotoxy(int x,int y)
{
    coord.X=x;
    coord.Y=y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),coord);
}

void window(int,int,int,int);


int num_check(char num)
	{
		int i=0,j=0;
		char N[6]={'1','2','3','4','5','6'};
		for(i=0;i<10;i++)
		{
			if(num==N[i])
			return 1;
		}
		return 0;
	}
	
class book
{
private:
	char serialno[20];
	char bookname[150];
	char authorname[150];
	char publisher[150];
	float price;
	int quantity;
	public:
	void getdata()
	{
		cout<<"enter serial no:";
		cin.ignore();
			cin.getline(serialno,20);
			cout<<"Enter book name: ";
			cin.ignore();
			cin.getline(bookname,150);
				cout<<"Enter author name: ";
				cin.ignore();
			cin.getline(authorname,150);
				cout<<"Enter publisher  name: ";
				cin.ignore();
			cin.getline(publisher,150);
			
		cout<<"Enter price: ";
			cin>>price;
			cout<<"Enter quantity: ";
			cin>>quantity;
		
	}
	void add()
	{
		char ch[5];
	char no[3]={'n','o'};
    char n[2]={'n'};
    char N[2]={'N'};
    char NO[3]={'N','O'};
    char nO[3]={'n','O'};
    char No[3]={'N','o'};
    char z[2]={'0'};
	ofstream fout;
	while(1)
	{
	fout.open("new.dat",ios::app|ios::binary);
	getdata();
	fout.write((char*)this,sizeof(*this));
	
	cout<<"do you want to add more books\n";
		cin>>ch;
		cout<<endl;
	     
    	cout<<endl;
  		cin.ignore();// https://stackoverflow.com/questions/6649852/getline-not-working-properly-what-could-be-the-reasons	
        if((strcmp(ch,n))==0)
   		{
     		break;	
		}
 	    else if((strcmp(ch,no))==0)
   		{
     		 break;	
		}
  	   
    	else if((strcmp(ch,N))==0)
    	{
     		break;	
		}
    	 else if((strcmp(ch,NO))==0)
    	{
     		break;	
		}
    	 else if((strcmp(ch,nO))==0)
		{
     		break;	
		}
	     else if((strcmp(ch,No))==0)
		{
     		break;	
		}
		 else if((strcmp(ch,z))==0)
		{
     		break;	
		}
		else 
		continue;	
	}
  fout.close();
	}	
void showBookData()
{
	cout<<"serial no: "<<serialno<<endl;
	cout<<"Book name: "<<bookname<<endl;
	cout<<"Author name: "<<authorname<<endl;
	cout<<"Publisher name: "<<publisher<<endl;
	cout<<"Price: "<<price<<endl;
	cout<<"Quantity: "<<quantity<<endl<<endl;
}	
    void	search(char* sno)
{
	int count=0;
	ifstream fin;
	fin.open("new.dat",ios::in|ios::binary);
	if(!fin)
	cout<<"\nFile not found";
	else {
		fin.read((char*)this,sizeof(*this));
		while(!fin.eof())
		{
			if(!strcmp(sno,serialno))
			{
				showBookData();
				count++;
			}
			fin.read((char*)this,sizeof(*this));
		}
		if(count==0)
		cout<<"\nrecord not found!\n";
		fin.close();
	}
} 
	void update(char *sno)
	{
	fstream f;
	f.open("new.dat",ios::in|ios::out|ios::ate|ios::binary);
	f.seekg(0);
	f.read((char*)this,sizeof(*this)) ;
	while(!f.eof())
	{
		if(!strcmp(sno,serialno))
		{
			cout<<"Enter change: "<<endl;
			getdata();
			f.seekp(f.tellp() - sizeof(*this));
			f.write((char*)this,sizeof(*this));
			cout<<"file updated: ";
			break;
		}
		else
		{
		f.read((char*)this,sizeof(*this));
	    }
	}
	f.close();
	}
	void dellet(char *sno)
	{
		ifstream fin;
		ofstream fout;
		fin.open("new.dat",ios::in|ios::binary);
		if(!fin)
	{
		cout<<"file not found: ";
	}
	else
	{
		fin.open("new1.dat",ios::out|ios::binary);
		fin.read((char*)this,sizeof(*this));
		while(!fin.eof())
		{
			if(!strcmp(sno,serialno))
			{
	
			}
			else
			{
				fout.write((char*)this,sizeof(*this));
				fin.read((char*)this,sizeof(*this));
			}
		}
		fin.close();
		fout.close();
		remove("new.dat");
		rename("new1.dat","new.dat");
	}
   }
   
};


class allrecord: public book{
	public:
		char c;
		ifstream i;
		void All()
		{
	    i.open("new.txt",ios::in|ios::binary);
		while(!i.eof())
		{
			showBookData();
			i.read((char*)this,sizeof(*this));
		}
	}
};

class billing: public allrecord{

};
int menu()
{
	char ch;
	cout<<"Enter choice: ";
	cin>>ch;
	return ch;
}


int main(void)
{
	int i;
	system("color E");
	book b;
	allrecord a;
	billing B;
	char serialno[20];
	char choice;
	char ch;
	const char *men[]= {"   Add Books","   Search Books ","   Update Books",  "   Delete Books ","   Display Books", "   Billing"};
	//cout<<"Enter 1 if you want to add books: "<<endl;
	//cout<<"Enter 2 if you want to search book: "<<endl;
	//cout<<"Enter 3 if you want to update book:  "<<endl;
	//cout<<"Enter 4 if you want to delete book: "<<endl;
	//cout<<"Enter 5 if you want to display books: "<<endl;
	
	window(25,50,20,32);
    gotoxy(33,18);
    printf("MAIN MENU");
    for (i=0; i<=5; i++)
    {
        gotoxy(30,22+i+1);
        printf("%s\n\n\n",men[i]);
    }
    cout<<endl<<endl<<endl<<endl<<endl;
	try{
	ch=menu();
	if(num_check(ch)==0)
	throw invalid_argument("\nEnter valid choice\n");
}
catch(invalid_argument &e)
{
	cerr<<"\n\nException while giving choice"<<e.what()<<endl;
}
	switch(ch)
	{
		case '1':
			b.add();
			break;
		case '2':
			
        	do
	{
	cout<<"enter serial no to search: ";
	cin.ignore();
	cin.getline(serialno,20);
	b.search(serialno);
	cout<<"do you want to search more: ";
	cin>>choice;
    }while(choice=='Y'||choice=='y');
		case '3':
			do
	{
	cout<<"enter serialno to modify: ";
	cin.ignore();
	cin.getline(serialno,20);
	b.update(serialno);
	cout<<"do you want to update more: ";
	cin>>choice;
    }while(choice=='Y'||choice=='y');
			break;
		case '4':
			do
	{
	cout<<"enter serial no to delete: ";
	cin.getline(serialno,20);
	b.dellet(serialno);
	cout<<"do you want to delete more books: ";
	cin>>choice;
    }while(choice=='Y'||choice=='y');
			break;	
			
			
	case '5':  // all record
	  a.All();			
	  break;
	  
	 case '6':
	   B.All();

	  do{
	  	 cout<<"Enter serial no: ";
	  	 cin.ignore();
	  	 cin.getline(serialno,20);
	   B.search(serialno);
	   cout<<"do you want to buy more books: ";
	cin>>choice;
	  }while(choice=='Y'||choice=='y');
	  
	   break; 
	}
    return(0);
    

}

/*function to display box*/
void window(int a,int b,int c,int d)
{
    int i;
    system("cls");
    gotoxy(20,10);
//textcolor(1);
    for (i=1; i<=7; i++)
        printf("*");
    printf(" * BOOK STORE * ");
    for (i=1; i<=7; i++)
        printf("*");
    printf("\n\n");
    gotoxy(25,11);
    printf("MANAGEMENT SYSTEM");
//textcolor(4);
    for (i=a; i<=b; i++)
    {
        gotoxy(i,17);
        printf("\xcd");
        gotoxy(i,19);
        printf("\xcd");
        gotoxy(i,c);
        printf("\xcd");
        gotoxy(i,d);
        printf("\xcd");
    }

    gotoxy(a,17);
    printf("\xc9");
    gotoxy(a,18);
    printf("\xba");
    gotoxy(a,19);
    printf("\xc8");
    gotoxy(b,17);
    printf("\xbb");
    gotoxy(b,18);
    printf("\xba");
    gotoxy(b,19);
    printf("\xbc");
//textcolor(4);
    for(i=c; i<=d; i++)
    {
        gotoxy(a,i);
        printf("\xba");
        gotoxy(b,i);
        printf("\xba");
    }
    gotoxy(a,c);
    printf("\xc9");
    gotoxy(a,d);
    printf("\xc8");
    gotoxy(b,c);
    printf("\xbb");
    gotoxy(b,d);
    printf("\xbc");
//textbackground(11);
//textcolor(0);
}
