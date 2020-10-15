# Bank-Management-System
Replicates a C++ Code For a Bank!


Code 
#include<iostream>   //library file used
#include<fstream>    //to get different outputs
#include<cctype>     //to use many functions
#include<iomanip>    //manipulate outcomes
#include<ctime>      //use system time in the console
#include<string>     //declare string
#include<cstdlib>


using namespace std;


class account            //declaring class and its content as public and private as access specifiers.
{
	int acno;

	char name[50];

	int deposit;
	char type;
public:
	void create_account();
	void show_account() const;
	void modify();
	void dep(int);
	void draw(int);
	void report() const;
	int retacno() const;
	int retdeposit() const;
	char rettype() const;

};

void account::create_account()     //create account statements
{
    cout << "Your Account Number is  = ";

	cin>>acno ;

	cout<<"\n\nEnter The Name of The account Holder : ";     //cout statement for name

	cin.ignore();

	cin.getline(name,50);                                    //can use unformatted string

	cout<<"\nEnter Type of The account (C/S) : ";            //cout for type

	cin>>type;

	type=toupper(type);                                      //type feature

	cout<<"\nEnter The Initial amount (Deposit To our Attendant) : ";    //initial amount

	cin>>deposit;



	cout<<"\nWith BMS now have a single account for GENERAL as well DEMAT Account:))\n";

	cout<<"\nMAY THE TRADING GODS BE WITH YOU!";

	cout<<"\n\n\nAccount Created..";                                                   //account generated
}

void account::show_account() const                                         //show account using scope declaration
{

	cout<<"\nAccount No. : "<<acno;                                   //declaring acno values get by  to acno

	cout<<"\nAccount Holder Name : ";

	cout<<name;

	cout<<"\nType of Account : "<<type;                    //declaring type to type

	cout<<"\nBalance amount : "<<deposit;                 //declaring balance to balance
}


void account::modify()                                // statements of cin,cout for modify account like acno,accholdername,type,balance
{
	cout<<"\nAccount No. : "<<acno;

	cout<<"\nModify Account Holder Name : ";

	cin.ignore();

	cin.getline(name,50);

	cout<<"\nModify Type of Account : ";

	cin>>type;

	type=toupper(type);

	cout<<"\nModify Balance amount : ";

	cin>>deposit;
}


void account::dep(int x)
{

	deposit+=x;

}

void account::draw(int x)     //withdraw
{

	deposit-=x;

}

void account::report() const                     //report
{

	cout<<acno<<setw(10)<<" "<<name<<setw(10)<<" "<<type<<setw(6)<<deposit<<endl;      //manipulator used setw

}

//Using scope resolution to define function outside the class
int account::retacno() const           //acno
{

	return acno;

}

int account::retdeposit() const         //deposit
{

	return deposit;

}

char account::rettype() const         //type
{

	return type;

}







void write_account();                    //declaring write_account for making new account

void display_sp(int);                    //declaring display_sp for balance enquiry

void modify_account(int);                //declaring modify_account for modifying account

void delete_account(int);                //declaring delete_account for deleting account

void deposit_withdraw(int, int);         //declaring deposit_withdraw for depositing and withdrawing

void markets();

void loan();

void intro();                            //declaring intro for introduction screen

void ipo();








int main()
{
	char ch;              //declaring things in the menu
	int num;
	intro();
	do
	{
		cout<<"\n\n\n\tMAIN MENU";              //the following things are the things present in the main menu

		cout<<"\n\n\t01. NEW ACCOUNT";

		cout<<"\n\n\t02. DEPOSIT AMOUNT";

		cout<<"\n\n\t03. WITHDRAW AMOUNT";

		cout<<"\n\n\t04. BALANCE ENQUIRY";

		cout<<"\n\n\t05. CLOSE AN ACCOUNT";

		cout<<"\n\n\t06. MODIFY AN ACCOUNT";

		cout<<"\n\n\t07. MARKETS";

		cout<<"\n\n\t08. LOAN Enquiry ";

		cout<<"\n\n\t09. IPO";



		cout<<"\n\n\t10. Exit ";

		cout<<"\n\n\tSelect Your Option (1-8) ";

		cin>>ch;

		switch(ch)                                   //using switch case for using a single operation at a time
		{
		case '1':
			write_account();
			break;
		case '2':
			cout<<"\n\n\tEnter The account No. : "; cin>>num;
			deposit_withdraw(num, 1);
			break;
		case '3':
			cout<<"\n\n\tEnter The account No. : "; cin>>num;       //function overloaded
			deposit_withdraw(num, 2);
			break;
		case '4':
			cout<<"\n\n\tEnter The account No. : "; cin>>num;
			display_sp(num);
			break;
        case '5':
			cout<<"\n\n\tEnter The account No. : "; cin>>num;
			delete_account(num);
			break;
		 case '6':
			cout<<"\n\n\tEnter The account No. : "; cin>>num;
			modify_account(num);
			break;
         case'7':
            markets();
            break;
         case'8':
             loan();
             break;
         case'9':
            ipo();
            break;
		 case '10':
			cout<<"\n\n\tThanks for using bank managemnt system";
			break;
		 default :cout<<"\a";
		}

		cin.ignore();                                //select one and ignore others

		cin.get();                                   //use the one selected
	}while(ch!='10');                                 // exit the program when selected 7
	return 0;
}



void write_account()                                 //to open new account
{
    time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);



	account ac;

	ofstream outFile;

	outFile.open("account.dat",ios::binary|ios::app);          //ios::app to perform at the end of the file

	ac.create_account();

	outFile.write(reinterpret_cast<char *> (&ac), sizeof(account));

	outFile.close();
}


void display_sp(int n)
{
    time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);




	account ac;

	bool flag=false;

	ifstream inFile;
	inFile.open("account.dat",ios::binary);                  //ios::app to perform at the end of the file

	if(!inFile)

	{
		cout<<"File could not be open !! Press any Key...";
		return;
	}
	cout<<"\nBALANCE DETAILS\n";

    	while(inFile.read(reinterpret_cast<char *> (&ac), sizeof(account)))
	{
		if(ac.retacno()==n)
		{
			ac.show_account();
			flag=true;
		}
	}

	inFile.close();
	if(flag==false)
		cout<<"\n\nAccount number does not exist";
}

void modify_account(int n)
{
    time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);




	bool found=false;

	account ac;


	fstream File;

	File.open("account.dat",ios::binary|ios::in|ios::out);

	if(!File)
	{
		cout<<"File could not be open !! Press any Key...";
		return;
	}
	while(!File.eof() && found==false)
	{
		File.read(reinterpret_cast<char *> (&ac), sizeof(account));

		if(ac.retacno()==n)
		{

			ac.show_account();

			cout<<"\n\nEnter The New Details of account"<<endl;

			ac.modify();

			int pos=(-1)*static_cast<int>(sizeof(account));

			File.seekp(pos,ios::cur);

			File.write(reinterpret_cast<char *> (&ac), sizeof(account));

			cout<<"\n\n\t Submit your documents to your nearest branch by ";

			found=true;

		  }
	}
	File.close();
	if(found==false)
		cout<<"\n\n Record Not Found ";
}



void delete_account(int n)
{
    time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);



	account ac;

	ifstream inFile;

	ofstream outFile;

	inFile.open("account.dat",ios::binary);

	if(!inFile)

	{

		cout<<"File could not be open !! Press any Key...";

		return;
	}

	outFile.open("Temp.dat",ios::binary);

	inFile.seekg(0,ios::beg);
	while(inFile.read(reinterpret_cast<char *> (&ac), sizeof(account)))
	{
		if(ac.retacno()!=n)
		{
			outFile.write(reinterpret_cast<char *> (&ac), sizeof(account));
		}
	}

	inFile.close();

	outFile.close();

	remove("account.dat");

	rename("Temp.dat","account.dat");

	cout<<"\n\n\t Submit CHEQUE BOOK and all other documents with identity proof";
}




void deposit_withdraw(int n, int option)
{

    time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);




	int amt;

	bool found=false;

	account ac;

	fstream File;

	File.open("account.dat", ios::binary|ios::in|ios::out);

	if(!File)
	{
		cout<<"File could not be open !! Press any Key...";
		return;
	}
	while(!File.eof() && found==false)
	{

		File.read(reinterpret_cast<char *> (&ac), sizeof(account));

		if(ac.retacno()==n)
		{
			ac.show_account();
			if(option==1)
			{

				cout<<"\n\n\tTO DEPOSIT AMOUNT ";

				cout<<"\n\nEnter The amount to be deposited";

				cin>>amt;

				ac.dep(amt);
			}

			if(option==2)

			{

				cout<<"\n\n\tTO WITHDRAW AMOUNT ";

				cout<<"\n\nEnter The amount to be withdraw";

				cin>>amt;

				int bal=ac.retdeposit()-amt;


				if((bal<500 && ac.rettype()=='S') || (bal<1000 && ac.rettype()=='C'))

					cout<<"Insufficience balance";

				else
					ac.draw(amt);
			}

			int pos=(-1)*static_cast<int>(sizeof(ac));

			File.seekp(pos,ios::cur);

			File.write(reinterpret_cast<char *> (&ac), sizeof(account));

			cout<<"\n\n\t Record Updated";

			found=true;
	       }
         }

	File.close();

	if(found==false)
		cout<<"\n\n Record Not Found ";
}

void markets()
{

    int quantity;

    float upperrange;

    float lowerrange;


    string exchange;

    string name;

    float execprice;

    int jack;

    float chintz;

    int demat;


     time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);




    cout<<"Enter Your Demat account number ";

    cin>>demat;



    cout<<"Enter exchange name \n";




    cin>>exchange;

    cout<<"Enter name of the company \n";

    cin>>name;

    cout<<"Kindly provide the range of +5 or -5\n";

    cout<<"Enter upper range \n";

    cin>>upperrange;

    cout<<"Enter lower range \n";

    cin>>lowerrange;

    cout<<"Quantity of Shares \n";

    cin>>jack;





    execprice=upperrange+lowerrange;



    execprice=execprice/2;


    cout<<"################################# \n";

    cout<<" \n Order Summary \n";

    cout<<" \n Name of exchange:  "<<exchange;

    cout<<" \n Name: "<<name;

    cout<<" \n order execution price:"<<execprice;



    chintz=jack*execprice;


    cout<<" \nTotal Investment  "<<chintz;


    cout<<" \n################################# \n";

}

void ipo()
{
         time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);


    int price;
    int qty;
    string name;
    int total;

    cout<<"\n Enter IPO company: \n";
    cin>>name;
    cout<<"Enter ISSUE price(LOT SIZE = 40): \n";
    cin>>price;
    cout<<"Enter Quantity : \n ";
    cin>>qty;

    total = price*qty;

    cout<<"\n####################################################\n";
    cout<<"\n Order Summary \n";
    cout<<"\n Issue Name : "<<name;
    cout<<"\n Issue price : "<<price;
    cout<<"\n Quantity : "<<qty;
    cout<<"\n Bill Amount : "<<total;
    cout<<"\n####################################################\n";


}

void loan()
{

     time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);



    float time;

    float rate;

    float principal;

    float emi;



    cout<<"Enter Principal Amount : \n ";

    cin>>principal;


    cout<<"Enter rate of Interest: \n ";

    cin>>rate;



    cout<<"Enter number of months : \n ";

    cin>>time;

    emi=rate/time*principal;

    cout<<"Your Monthly EMI is "<<emi;

}


void intro()
{
     time_t tt;


    struct tm * ti;

    time (&tt);

    ti = localtime(&tt);

    cout << "                                                                                             "
         << asctime(ti);



	cout<<"\n\n\n\t WORLD BANK ";

	cout<<"\n\n\t WELCOMES YOU TO THE MOST ADVANCE ";


	cout<<"\n\n\t BANK MANAGEMENT SOFTWARE";

	cout<<"\n\n\n\n DEVELOPED BY ";

	cout<<"\n\n\n                -CHINTZ RUPAREL";




	cin.get();




}


