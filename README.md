# Student-database
package StuRank_pack;
import StuGrade_pack.StudentGrade;
class rankException extends Exception
{
    rankException(String str)
    {
        super(str);
    }
}
public class Rank extends StudentGrade
{
    public Rank(String x , String y , int z)
    {
        super(x,y,z);
    }
    public void rank_cal(StudentGrade arr[],int n)
    {
        int i,j;
        StudentGrade temp;
        for(i=0;i<n;i++)
        {
            for(j=i+1;j<n;j++)
            {
                if(arr[i].tot<arr[j].tot)
                {
                    temp=arr[i];
                    arr[i]=arr[j];
                    arr[j]=temp;
                }
            }
        }
        int rank_temp=0;
        for(i=0;i<n;i++)
        {
            if(i==0)
            {
                rank_temp++;
                arr[i].rank=rank_temp;
            }
            if(i>0)
            {
                if(arr[i-1].tot==arr[i].tot)
                {
                    arr[i].rank=arr[i-1].rank;
                    rank_temp++;
                }
                else
                {
                    rank_temp++;
                    arr[i].rank=rank_temp;
                }
            }
            System.out.println(arr[i].rank+"\t"+arr[i].name+"\t"+arr[i].tot);
        }
        for(i=0;i<n;i++)
        {
            try
            {
                if(arr[i].rank>3)
                {
                    throw new rankException("Not enough to Get a rank");
                }
                System.out.println("No rankException for"+arr[i].name);
            }
            catch(rankException obj)
            {
                System.out.println("Rank Exception caught for rank"+arr[i].name+" "+obj);
            }
        }
    }
}
package StuGrade_pack;
import StudentInterface_pack.Student;
import java.util.*;
class attendanceException extends Exception 
{
    attendanceException(String str)
    {
        super(str);
    }
}
class gpaException extends Exception
{
    gpaException(String str)
    {
        super(str);
    }
}
public class StudentGrade implements Student
{
    public String name,city;
    public int age,tot,rank,mark=0;
    public StudentGrade(String x, String y, int z)
    {
        name=x;
        city=y;
        age=z;
    }
    Scanner kb=new Scanner(System.in);
    public void attendanceCal()
    {
        int abs,tot;
        System.out.println("Enter The Total number of hours present");
        tot=kb.nextInt();
        System.out.println("Enter the Total number of hours absent");
        abs=kb.nextInt();
        try
        {
            float attend=((float)(tot-abs)/tot)*100;
            System.out.println("Attendance is "+attend);
            if(attend>90)
            {
                mark=5;
            }
            else if(attend > 85 && attend <= 90)
            {
                mark=4;
            }
            else if(attend > 80 && attend <= 85)
            {
                mark=3;
            }
            else if(attend > 70 && attend <= 80)
            {
                mark=2;
            }
            else if(attend >= 60 && attend <= 70)
            {
                mark=1;
            }
            else if(attend < 60)
            {
                mark=0;
            }
        }
        catch(ArithmeticException obj)
        {
            System.out.println("Arithmetic Excpetion has been catched"+obj);
        }
        System.out.println("\n The attendance mark is "+mark);
        try
        {
            if(mark<2.5)
            {
                throw new attendanceException("Low Mark, Classes to be attended regularly");
            }
        }
        catch(attendanceException obj)
        {
            System.out.println("Attendance Exception has been caught"+obj);
        }
    }
    String grade;
    public int calculateGrade()
    {
        int flag=0,i;
        float avg;
        Scanner kb=new Scanner(System.in);
        int[] mark_data = new int[5];
        System.out.println("Enter the marks of the five subjects(out of 95)");
        for(i=0;i<5;i++)
        {
            mark_data[i]=kb.nextInt();
            if(mark_data[i]>95)
            {
                i--;
            }
            else
            {
                tot=tot+mark_data[i];
                if(mark_data[i]<50)
                {
                    flag++;
                }
            }
        }
        try
        {
            if(flag>0)
            {
                System.out.println("Grade is not applicable");
                throw new gpaException("Not Eligible for GPA");
            }
            else
            {
                tot+=(5*mark);
                avg=tot/5;
                if(avg > 90)
                {
                    grade="O";
                }
                else if(avg > 85 && avg <= 90)
                {
                    grade="A+";
                }
                else if(avg > 80 && avg <= 85)
                {
                    grade="A";
                }
                else if(avg > 70 && avg <= 80)
                {
                    grade="B+";
                }
                else if(avg > 60 && avg <= 70)
                {
                    grade="B";
                }
                else if(avg >= 50 && avg <= 60)
                {
                    grade="C";
                }
                else if(avg < 50)
                {
                    grade="F";
                }
                System.out.println("Grade is :"+grade);
            }
        }
        catch(gpaException obj)
        {
            System.out.println("Grade Exception has been caught"+obj);
        }
        return tot;
    }
    public void printData()
    {
        System.out.println("The name of the Student is"+name);
        System.out.println("The city of the Student is "+city);
        System.out.println("The age of the Student is "+age);
    }
}
package StudentInterface_pack;
import java.util.*;
public interface Student 
{
    void printData();
}
interface Attendance extends Student
{
    void attendanceCal();
}
interface Grade extends Student
{
    int calculateGrade();
}
package SearchStudent_pack;
import java.util.*;
import DisplayStudent_pack.DisplayStudent;
import StuGrade_pack.StudentGrade;
public class SearchStudent extends DisplayStudent
{
    public SearchStudent(String x,String y, int z)
    {
        super(x,y,z);
    }
    public void searchstud(StudentGrade arr[],int n)
    {
        int ch,x=1,i;
        char ctemp;
        while(x!=0)
        {
            System.out.println("Enter 1:Search the student name for the given starting letter");
            System.out.println("Enter 2:Search the city name for the given ending letter");
            System.out.println("Enter 3: Search the student with the name given");
            System.out.println("Enter 4:Get the name of the students with given substring");
            System.out.println("Enter 5:Search Shortest and the Longest Student name");
            String stemp1,stemp2;
            Scanner kb=new Scanner(System.in);
            int flag=0;
            System.out.println("Enter your choice");
            ch=kb.nextInt();
            switch(ch)
            {
                case 1:
                {
                    System.out.println("Enter the Starting Letter");
                    ctemp=kb.next().charAt(0);
                    for(i=0;i<n;i++)
                    {
                        if(ctemp==arr[i].name.charAt(0))
                        {
                            System.out.println(arr[i].name);
                        }
                        else
                        {
                            flag++;
                        }
                    }
                    if(flag==n)
                    {
                        System.out.println("Not Found");
                    }
                    break;
                }
                case 2:
                {
                    System.out.println("Enter the Ending Letter");
                    ctemp=kb.next().charAt(0);
                    for(i=0;i<n;i++)
                    {
                        if(ctemp==arr[i].city.charAt(arr[i].city.length()-1))
                        {
                            System.out.println(arr[i].city);
                        }
                        else
                        {
                            flag++;
                        }
                    }
                    if(flag==n)
                    {
                        System.out.println("Not found");
                    }
                    break;
                }
                case 3:
                {
                    flag=0;
                    System.out.print("Enter the Student Name for searching:");
                    Scanner sc1= new Scanner(System.in);
                    stemp1=sc1.nextLine();
                    for(i=0;i<n;i++)
                    {
                        try
                        {
                            if(stemp1.equals(arr[i].name))
                            {
                                System.out.println("Found!");
                                break;
                            }
                            else
                                flag++;
                        }
                        catch(NullPointerException e_obj)
                        {
                            System.out.println("Null pointer Exception catched! -> "+e_obj);
                        }
                    }
                    if(flag==n)
                    {
                        System.out.println("Not Found!");
                        break;
                    }
                    break;
                }
                case 4:
                {
                    flag=0;
                    System.out.print("Enter the substring for searching:");
                    Scanner sc2= new Scanner(System.in);
                    stemp2=sc2.nextLine();
                    for(i=0;i<n;i++)
                    {
                        if(arr[i].name.contains(stemp2))
                        {
                            System.out.println("Found in "+ arr[i].name);
                        }
                        else
                            flag++;
                    }
                    if(flag==n)
                    {
                        System.out.println("Not Found!");
                    }
                    break;
                }
                case 5:
                {
                    int c=0;
                    if(n>2)
                    {
                        StudentGrade temp_obj;
                        for(i=0;i<n-1;i++)
                        {
                            if((arr[i].name).length()>(arr[i+1].name).length())
                            {
                                temp_obj=arr[i];
                                arr[i]=arr[i+1];
                                arr[i+1]=temp_obj;
                                i=0;
                            }
                        }
                        if(arr[0].name.length()==arr[1].name.length())
                        {
                            for(i=1;i<n;i++)
                            {
                                if(arr[i].name.length()==arr[0].name.length())
                                {
                                    System.out.println("Shortest name: " + arr[i].name);
                                }
                                c++;
                            }
                            if(c==n)
                            {
                                for(i=0;i<n;i++)
                                {
                                    System.out.println("Longest name: " + arr[i].name);
                                }
                            }
                            else
                                System.out.println("Longest name: " + arr[n-1].name);
                        }
                        else
                        {
                            System.out.println("Shortest name: "+arr[0].name+"\nLongest name: " + arr[n-1].name);
                        }
                    }
                    else if(n==2)
                    {
                        System.out.println("Shortest name: "+arr[0].name+","+arr[1].name+"\nLongest name: " + arr[0].name+","+arr[1].name);
                    }
                    else
                    {
                        System.out.println("Shortest name: "+arr[0].name+"\nLongest name: " + arr[n-1].name);
                    }
                    break;
                }
                default:
                {
                    System.out.print("Invalid Choice!");
                    break;
                }
            }
            System.out.println("Enter 1: To continue");
            System.out.println("Enter 2: To Stop");
            x=kb.nextInt();
            if(x==2)
            {
                break;
            }
        }
    }
}
package MainStudentGrade;
import java.util.*;
import SearchStudent_pack.SearchStudent;
import DisplayStudent_pack.DisplayStudent;
import StuGrade_pack.StudentGrade;
import StuRank_pack.Rank;
public class Main
{
    public static void main(String args[])
    {
        String name,city;
        int i,j,n,age;
        Scanner kb=new Scanner(System.in);
        System.out.println("Enter The database for the Students to be created");
        n=kb.nextInt();
        StudentGrade arr[]=new StudentGrade[n];
        int tot_arr[]=new int[n];
        for(i=0;i<n;i++)
        {
            Scanner x=new Scanner(System.in);
            System.out.println("Enter the details for the object"+(i+1)+":");
            System.out.print("Enter the name:");
            name=x.nextLine();
            System.out.print("Enter the city:");
            city=x.nextLine();
            System.out.print("Enter the age:");
            age=x.nextInt();
            arr[i]=new StudentGrade(name,city,age);
			arr[i].attendanceCal();
			try
            {
                tot_arr[i]=arr[i].calculateGrade();
            }
            catch(ArrayIndexOutOfBoundsException obj)
            {
                System.out.println("Array out of bound Exception catched"+obj);
            }
        }
        Rank r=new Rank(" "," ",0);
        r.rank_cal(arr,n);
        DisplayStudent d = new DisplayStudent(" "," ",0);
        for(i=0;i<n;i++)
        {
            System.out.print("For Student "+(i+1)+":");
            arr[i].printData();    
            d.display_student(arr[i]);
        }
        SearchStudent s =new SearchStudent(" "," ",0);
        s.searchstud(arr,n);
    }
}package DisplayStudent_pack;
import java.util.*;
import StuRank_pack.Rank;
import StuGrade_pack.StudentGrade;
public class DisplayStudent extends Rank
{
    public DisplayStudent(String x, String y, int z)
    {
        super(x,y,z);
    }
    public void display_student(StudentGrade obj)
    {
        Scanner kb=new Scanner(System.in);
        int ch;
        System.out.println("1-Display the name and city with a capital Letter");
        System.out.println("2-Display the name and city with Lowercase");
        System.out.println("3-Display the name and city in Uppercase");
        System.out.println("4-No change");
        ch=kb.nextInt();
        switch(ch)
        {
            case 1:
            {
                System.out.println("Name:"+obj.name.substring(0,1).toUpperCase() + obj.name.substring(1) +"\nCity:"+obj.city.substring(0,1).toUpperCase()+obj.city.substring(1));
                break;
            }
            case 2:
            {   
                System.out.println("Name:"+obj.name.toLowerCase()+"\nCity:"+obj.city.toLowerCase());
                break;
            }
            case 3:
            {   System.out.println("Name:"+obj.name.toUpperCase()+"\nCity:"+obj.city.toUpperCase());
                break;
            }
            case 4:
            {
                System.out.println("Name:"+obj.name+"\nCity:"+obj.city);
                break;
            }
            default:
                System.out.println("Invalid Choice!");
                break;
        }
    }
}
