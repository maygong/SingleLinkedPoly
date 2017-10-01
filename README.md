# SingleLinkedPoly
public class SingleLinkedPoly implements Polynomial{
   
   private static class IntDouObject
   {
      private int integer;
      private double dou;
      public IntDouObject(int a,double b){
         integer=a;
         dou=b;
      }
      public int getInt(){
         return integer;
      }
      public double getDouble(){
         return dou;
      }
      public void setInt(int a){
         integer=a;
      }
      public void setDouble(double b){
         dou=b;
      }
   }//inner-class
   private IntDouObject value;
   private SingleLinkedPoly next;
   public SingleLinkedPoly(IntDouObject initValue,SingleLinkedPoly initNext)
   { 
      value = initValue; 
      next = initNext; 
   } 
   public IntDouObject getValue() 
   { 
      return value; 
   }    
   
   public SingleLinkedPoly getNext() 
   { 
      return next;  
   } 
   
   public void setValue(IntDouObject theNewValue)
   { 
      value = theNewValue;
   }
   
   public void setNext(SingleLinkedPoly theNewNext)
   { 
      next = theNewNext; 
   }
   
   public static void main(String[]args)
   {  
      SingleLinkedPoly test1=new SingleLinkedPoly(new IntDouObject(1,1),null);
      
      SingleLinkedPoly test2=new SingleLinkedPoly(new IntDouObject(2,3),null);
      test2=new SingleLinkedPoly(new IntDouObject(1,2),test2);
      test2=new SingleLinkedPoly(new IntDouObject(0,1),test2);
      
            
      System.out.println(test1);
      System.out.println(test2);
      System.out.println(test2.equals(test1));
      System.out.println(test2.plus(test1));
      System.out.println(test2.minus(test1));
      System.out.println(test1.multiply(test2));
   }
   
   //This method takes one parameter of type SingleLinkedPoly
   //It checks if there's any term of other has the same degree with the original one and combine their coefficient
   //If not, then the term will be added at the end of the original SingleLinkedPoly.
   public Polynomial plus(Polynomial other)
   {
      SingleLinkedPoly oo=(SingleLinkedPoly)other;
      SingleLinkedPoly o=oo;
      SingleLinkedPoly plus=this;
      SingleLinkedPoly pointer=plus;
      SingleLinkedPoly rear=pointer;
      SingleLinkedPoly removed=oo;
      while(pointer!=null){
         rear=pointer;
         pointer=pointer.getNext();
      }
      if(plus == null)
         return o;
      else{
         boolean has=false;
         for(SingleLinkedPoly pointer1=plus;pointer1!=null;pointer1=pointer1.getNext()){
            int expo=o.getValue().getInt();
            for(SingleLinkedPoly pointer2=o;pointer2!=null;pointer2=pointer2.getNext()){
               if(expo==pointer1.getValue().getInt()){
                  pointer1.setValue(new IntDouObject(expo,pointer1.getValue().getDouble()+pointer2.getValue().getDouble()));
                  //removed=remove(oo,pointer2.getValue());
                  //has=true;
               }//if  
            }//for-pointer2 
         }//for-pointer1
         //if(has!=true) return plus;
         //else rear.setNext(removed);
      }
      return plus;
   }
   
   //This one is basically the same logic as plus().
   //I'm going to fix plus first.
   
   //This method takes one parameter of type SingleLinkedPoly
   //It checks if there's any term of other has the same degree with the original one and subtract their coefficient
   //If not, then the term with a sign of "-" will be added at the end of the original SingleLinkedPoly.
   public Polynomial minus(Polynomial other)
   {
      SingleLinkedPoly o=(SingleLinkedPoly)other;
      SingleLinkedPoly minus=this;
      if(minus == null)
         return o;
      else{
         int expo=o.getValue().getInt();
         for(SingleLinkedPoly pointer1=minus;pointer1!=null;pointer1=pointer1.getNext()){
            for(SingleLinkedPoly pointer2=o;pointer2!=null;pointer2=pointer2.getNext()){
               if(expo==pointer1.getValue().getInt())
                  pointer1.setValue(new IntDouObject(expo,pointer1.getValue().getDouble()-pointer2.getValue().getDouble()));
            }//for1
         }//for2
         return minus;
      }//else
   }
   
   public Polynomial multiply(Polynomial other)
   {
      SingleLinkedPoly o=(SingleLinkedPoly)other;
      SingleLinkedPoly pointer=this;
      SingleLinkedPoly multiply=new SingleLinkedPoly(null,null);
      
      if(other==null||this==null)
         return null;
      /*
      else if(multiply==null){
         IntDouObject times=new IntDouObject(o.getValue().getInt()*this.getValue().getInt(),o.getValue().getDouble()*this.getValue().getDouble());
         multiply=new SingleLinkedPoly(times,null);
         o=o.getNext();
      }
      */
      else
         while(pointer!=null){
            while(o!=null){
               IntDouObject times=new IntDouObject(o.getValue().getInt()*pointer.getValue().getInt(),o.getValue().getDouble()*pointer.getValue().getDouble());
               multiply.plus(new SingleLinkedPoly(times,null));
            
            }//while
         }//while
         
      return multiply;
   }  
   
   public Polynomial differentiate (Polynomial other)
   {  
      SingleLinkedPoly o=(SingleLinkedPoly)other;
      SingleLinkedPoly diff=new SingleLinkedPoly(null,null);
      while(o!=null){
         IntDouObject term=new IntDouObject(o.getValue().getInt()-1,o.getValue().getDouble()*o.getValue().getInt());
         diff=new SingleLinkedPoly(term,diff);
         o=o.getNext();
      }
      return diff;
      
   }  
   public double evaluate(double x)
   {
      double result=0;
      SingleLinkedPoly temp=this;
      while(temp!=null){
         result+=temp.getValue().getDouble();
         temp=temp.getNext();
      }
      return result;
   }
   public String toString()
   {
      String finalPoly="";
      SingleLinkedPoly p = this;
      while(p!=null){
         if(p.getNext()==null)
            finalPoly=finalPoly+p.getValue().getDouble()+"x^"+p.getValue().getInt();
         else
            finalPoly=finalPoly+p.getValue().getDouble()+"x^"+p.getValue().getInt()+"+";
         p=p.getNext();
      }
      return finalPoly;
   }
   
public SingleLinkedPoly removeLast(SingleLinkedPoly  head)
   {	
      if(head==null)
         return null;
      else{
         SingleLinkedPoly secondrear=head;
         while(secondrear.getNext().getNext()!=null){
            secondrear=secondrear.getNext();
         }
         secondrear.setNext(null);
         return head;
      }
   }

   public SingleLinkedPoly remove(SingleLinkedPoly head, IntDouObject value)
   {	
         SingleLinkedPoly temp1=head;
         SingleLinkedPoly temp=head;
         SingleLinkedPoly rear=temp1;
         while(temp1!=null){
            rear=temp1;
            temp1=temp1.getNext();
         }
         if(head.getValue().equals(value))
            head=new SingleLinkedPoly(null,null);
         else if(rear.getValue()==value)
            head=removeLast(head);
         else{
            while(temp.getNext()!=null){
               if(temp.getNext().getValue()==value){
                 temp.setNext(temp.getNext().getNext());
               }
               temp=temp.getNext();
            }
         }//else
         return head;
   }
   /*
   public Boolean equals(Polynomial other)
   {
      SingleLinkedPoly o=(SingleLinkedPoly)other;
      SingleLinkedPoly term=this;
      while(term!=null){
         while(o!=null){   
            if(o.getValue().getInt()==term.getValue().getInt()&&o.getValue().getDouble()==term.getValue().getDouble())
               remove(o,o.getValue());
            }
            o=o.getNext();
         }//while-o
         term=term.getNext();
      }//while-term
      return o==null;
   }
   */
}

   
