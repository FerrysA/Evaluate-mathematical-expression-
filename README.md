# Evaluate mathematical expression-2kyu
# 16.12.2022
# [cылка на задание]( https://www.codewars.com/kata/52a78825cdfc2cfc87000005/train/java)
# Код:
```java
public class MathEvaluator {

static public double calculate(String s)
   {
       String Res="";
       s=format(s);

       int n=s.length();
       String[]S=new String[n];
           for (int i=0;i<n;i++)
           {
               S[i]=Character.toString(s.charAt(i));
           }

       int []L=new int[n];
       {
           {
               for(int i=0;i<n;i++)
               {
                   if(S[i].equals("("))
                   {
                       L[i]=1;
                   }
                   if(S[i].equals(")"))
                   {
                       L[i]=-1;
                   }

               }
           }
       }

       double [][]D=new double[n][2];
       {
           double[] DD=new double[2];

           int index0sush=0;
           for (int i=0;i<n;)
           {

                   if (S[i].equals("0") ||
                           S[i].equals("1") ||
                           S[i].equals("2") ||
                           S[i].equals("3") ||
                           S[i].equals("4") ||
                           S[i].equals("5") ||
                           S[i].equals("6") ||
                           S[i].equals("7") ||
                           S[i].equals("8") ||
                           S[i].equals("9") ||
                           S[i].equals(".")
                   ) {
                  DD=Rasp_num( S ,i);
                  D[i][0]=DD[0];
                  D[i][1]=i;if(i==0){index0sush=1;}
                  i=(int)DD[1];

                    }
                   else
                   {
                       D[i][1]=-1;
                       i++;

                   }
           }

           for(int i=0;i<n;i++)
           {
               if(i==0&&index0sush==0){ D[i][1]=-1;}
               if(D[i][1]==0&&i>0)
               {
                   D[i][1]=-1;
               }
           }

       }

       System.out.println(s);
      //Print(L);
     //  Print(D);
      // Print(S);
       //Calc(s,S,L,D);
       return Calc(s,S,L,D);
    }

    static public double Calc(String s,String[]S,int[]L,double[][]D)
    {
        // если протсая строка то калк про и аддд потом ретурн

        int n=S.length;
        int ind_S =0;// со скобкой включително
        int ind_F =0;// со скобкой включително
        /// поиск скобки
        {
            for(int i=0;i<n;i++)
            {
                if(L[i]==1)
                {
                    ind_S=i;
                }
                if(L[i]==-1)
                {
                    ind_F=i;
                    break;
                }
            }

        }


        if(ind_S ==0&&ind_F==0)
        {
            return CalcPro(s,S,L,D,-1,D.length);
        }
        double pro= CalcPro(s,S,L,D,ind_S,ind_F);


        int m=S.length-(ind_F-ind_S)-1;
        int[] L1 = new int[m+1];
        String S1[] = new String[m+1];
        double D1[][] =new double[m+1][2];

        {
            {
               /* int[] L2 = new int[L.length - ind_F];
                L2 = Arrays.copyOfRange(L, ind_F + 1, L.length);
                int[] L3 = new int[L.length-(ind_F-ind_S)];*/
                int j=0;
                for(int i=0;i<L.length;i++)
                {
                    if(i<ind_S)
                    {
                        L1[j]=L[i];
                        j++;
                        continue;
                    }
                    if(i==ind_S)
                    {
                        L1[j]=0;
                        j++;
                        continue;
                    }
                    if(i>ind_F)
                    {
                        L1[j]=L[i];
                        j++;
                        continue;
                    }
                }

            }// подрезание L
            {
                int j=0;
                for(int i=0;i<S.length;i++)
                {
                    if(i<ind_S)
                    {
                        S1[j]=S[i];
                        j++;
                        continue;
                    }
                    if(i==ind_S)
                    {
                        S1[j]=pro+"";
                        j++;
                        continue;
                    }
                    if(i>ind_F)
                    {
                        S1[j]=S[i];
                        j++;
                        continue;
                    }
                }

            }// подрезание S
            {
                int j=0;
                for(int i=0;i<D.length;i++)
                {
                    if(i<ind_S)
                    {
                        D1[j][0]=D[i][0];
                        D1[j][1]=D[i][1];
                        j++;
                        continue;
                    }
                    if(i==ind_S)
                    {
                        D1[j][0]=pro;
                        D1[j][1]=ind_S;
                        j++;
                        continue;
                    }
                    if(i>ind_F)
                    {
                        D1[j][0]=D[i][0];
                        D1[j][1]=D[i][1];
                        j++;
                        continue;
                    }
                }

            }// подрезание D

        }

       // Print(L1);
       // Print(S1);
       // Print(D1);
        return Calc(s,S1,L1,D1);

    }

    static public double CalcPro(String s1,String[]S,int[]L,double[][]D,int s,int f)
    {
        int dn=D.length;

        double[] Res=new double[f-s+1];

        int ir=0;

        String pred="";
        int sign=1;
        int SIGN[]=new int[f-s+1];
        {
            for (int i = s + 1; i < f; i++) {

                if (D[i][1] == -1 && i == s + 1 && S[i].equals("-")) {// минус впереди сразу после скобки
                    sign = -1;
                    continue;
                }
                if (D[i][1] != -1 && (pred.equals("") || pred.equals("+") || pred.equals("-"))) {
                    Res[ir] = sign * D[i][0];
                    sign = 1;
                    pred="";
                    continue;
                }
                if (D[i][1] != -1 && pred.equals("*")) {
                    Res[ir] *= D[i][0];
                    continue;
                }
                if (D[i][1] != -1 && pred.equals("/")) {
                    Res[ir] /= D[i][0];
                    continue;
                }
                if (S[i].equals("*")) {
                    pred = "*";
                }
                if (S[i].equals("/"))// после звездочек уперлись в +
                {
                    pred = "/";
                }
                if (S[i].equals("+")) {
                    pred = "+";
                    SIGN[ir] =1;
                    ir++;

                }
                if (S[i].equals("-")) {
                   /* if(pred.equals("*")||pred.equals("/"))
                    {
                        sign=-1;
                        continue;
                    }*/
                    pred = "+";
                    SIGN[ir] = -1;
                    ir++;
                }

            }

        }
        double Ress=0;


        pred="";
        Ress=Res[0];
        for (int i=1;i<f-s+1;i++)
        {

            if(SIGN[i-1]==1)
            {
                Ress+=Res[i];
                continue;
            }
            if(SIGN[i-1]==-1)
            {
                Ress-=Res[i];
                continue;
            }

        }
        System.out.println(Ress);
        return Ress;
    }

    public  static double[] Rasp_num(String[] S,int i)
    {
        int n=S.length;
        double[] Res=new  double[2];
        String Ress="";

        String sss=S[i];
        boolean Stop=true;
        while (Stop) {
            if (S[i].equals("0") ||
                    S[i].equals("1") ||
                    S[i].equals("2") ||
                    S[i].equals("3") ||
                    S[i].equals("4") ||
                    S[i].equals("5") ||
                    S[i].equals("6") ||
                    S[i].equals("7") ||
                    S[i].equals("8") ||
                    S[i].equals("9") ||
                    S[i].equals(".")
            ) {
                Ress += S[i];
                i++;
                if(i==n)
                {
                    Stop=false;
                }
            }
            else {
                Stop=false;
            }
        }



        Res[0] =Double.parseDouble(Ress);
        Res[1]=i;
        return Res;
    }
   public   static  String format(String s)
    {
        String Res;
         Res= s.replaceAll(" ", "");
         Res=Res.replaceAll("--", "+");


        Res=Res.replaceAll("\\-+", "-");
        Res=Res.replaceAll("\\++", "+");
        Res=Res.replaceAll("\\+-", "-");
        Res=Res.replace("-+", "-");
        Res=Res.replace("*-", "*(-1)*");
       Res=Res.replace("/-", "*(-1)/");
        System.out.println();
       // StringBuffer sb = new StringBuffer(s);
       // sb.insert(3,"123");
       // String ss=sb.toString();
        //System.out.println(ss);

        return Res;
    }
    public   static   void Print(int[] A)
    {
        int n=A.length;
        System.out.println();
        for(int i=0;i<n;i++)
        {
            System.out.print(A[i]);
        }
        System.out.println();
    }

    public static void Print(double[][]A)
    {
        int  m =A[0].length;//длина
        int n= A.length;// высота


                for(int j=0;j<n;j++)
                {

                    System.out.print("( "+A[j][0]+"/"+A[j][1]+" )");
                }




            System.out.println(" ");
        }
    public   static   void Print(String[] A)
    {
        int n=A.length;
        System.out.println();
        for(int i=0;i<n;i++)
        {
            System.out.print(A[i]);
        }
        System.out.println();
    }

}

```
# понравивщееся решение:(автор  ankr)
``` java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class MathEvaluator {

  private final Pattern parens = Pattern.compile("(-?)\\(([^()]+)\\)");
  private final Pattern divMul = Pattern.compile("(-?[0-9.]+)\\s*(/|\\*)\\s*(-?[0-9.]+)");
  private final Pattern addSub = Pattern.compile("(-?[0-9.]+)\\s*(\\+|-)\\s*(-?[0-9.]+)");

  public double calculate(String expression) {
    Matcher m;
    while ((m = parens.matcher(expression)).find()) {
      String eval = evaluate(m.group(2));
      if (!m.group(1).isEmpty())
        eval = eval.startsWith("-") ? eval.substring(1) : "-" + eval;
      expression = expression.substring(0, m.start()) + eval + expression.substring(m.end());
    }
    return Double.parseDouble(evaluate(expression));
  }
  
  private String evaluate(String expression) {
    Matcher m;
    while ((m = divMul.matcher(expression)).find()) {
      double x = Double.parseDouble(m.group(1));
      double y = Double.parseDouble(m.group(3));
      double v = m.group(2).equals("*") ? x * y : x / y;
      expression = expression.substring(0, m.start()) + v + expression.substring(m.end());
    }
    while ((m = addSub.matcher(expression)).find()) {
      double x = Double.parseDouble(m.group(1));
      double y = Double.parseDouble(m.group(3));
      double v = m.group(2).equals("+") ? x + y : x - y;
      expression = expression.substring(0, m.start()) + v + expression.substring(m.end());
    }
    return expression;
  }

}
