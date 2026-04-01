## Break
public class HelloWorld{

     public static void main(String []args){
        int[] array = {1,2,4,8,9};

        for (int i=0; i<array.length; i++) {
        	if (array[i] == 4) {
        		System.out.println("Index:"+i);
        		break;
        	}
        }
     }
}
## Continue
public class HelloWorld{

     public static void main(String []args){
        int[] array = {1,2,13,3};
        int S=0;

        for (int i:array) {
        	if (i == 13) {
        		continue;
        	}
        	S+=i;
        }
        System.out.print("Total:"+S);
     }
}
