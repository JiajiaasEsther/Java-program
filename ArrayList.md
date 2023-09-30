## ArrayList

```
/**
 * TODO: Add your file header
 * Name: Jiajia Li
 * ID: A17158450
 * Email: jil273@ucsd.edu
 * File description: This file is inclue the methods and setting about 
 * arraylist, and we need to add a method about reverseregion for it.
 */

/**
 * TODO: Add class header
 * This class is to set up the basic setting and method for the list to 
 * show like the arraylist. Also, it included the method about reverse.
 */
public class MyArrayList<E> implements MyReverseList<E> {
    static final int DEFAULT_CAPACITY = 5;
    
    Object[] data;
    int size;

    //IMPORTANT: DO NOT MODIFY THIS CONSTRUCTOR!
    //IMPORTANT: DO NOT ADD ANY MORE CONSTRUCTORS!

    /**
     * Constructor to create an array list with the given array's elements
     * @param arr - array of elements to be used to construct the ArrayList
     */
    public MyArrayList(E[] arr) {
        if (arr == null) {
            this.data = new Object[DEFAULT_CAPACITY];
        } else {
            this.data = arr.clone();
            this.size = arr.length;
        }
    }

    /**
	 * TODO: Method header comment here
     * The method the reverse the elements in the list 
     * between fromIndex and toIndex.
     * @param fromIndex - index of element that going to reverse from
     * @param toIndex - index of element that going to reverse to
	 */
    public void reverseRegion(int fromIndex, int toIndex){
       /**
        * TODO: Add your solution here
        */
        if(fromIndex > size - 1 || toIndex > size - 1 ||
         fromIndex < 0 || toIndex < 0){
            throw new IndexOutOfBoundsException();
        }
        if(fromIndex < toIndex){
            for(int i = fromIndex; i < toIndex; i++){
                //set a temp object to store data
                Object temp = data[i];
                //swith element 
                data[i] = data[toIndex];
                data[toIndex] = temp;
                //change position of toIndex to reverse with different i
                toIndex--;
            }
        }
    }

    @Override
    /**
     * A method that returns the number of valid elements
     * in the ArrayList 
     * @return - number of valid elements in the arraylist
     */
    public int size() {
        return this.size;
    }

    @Override
    /**
     * A method that returns an Element at the specified index
     * @param index - the index of the return Element
     * @return Element at specified index
     */
    @SuppressWarnings("unchecked")
    public E get(int index) {
        return (E) data[index];
    }
}
```



## ArrayList Reverse
```
/**
 * Tests to check the implementation of reverseRegion method in MyArrayList.java
*/
/**
 * Name: Jiajia Li
 * ID: A17158450
 * Email: jil273@ucsd.edu
 * File description:  This file is to test the method about reverseregion
 * for the arraylist.
 */

//other import statements

import org.junit.*;
import static org.junit.Assert.*;

//IMPORTANT: DO NOT MODIFY THE TEST HEADERS!
/**
 * This class contains various test cases to test the reverseRegion method
 */
public class ReverseArrayListTester {
    String[] arrEmptyStr = {};
    Integer[] arrSingle = {12};
    Integer[] arrInteger = {1, 2, 2, 3};
    String[] arrString = {"hey", "hi", "hello"};
    Integer[] listEmptyInt = {};
    String[] listString= {"hey", "hi", "hello", "bye"};

    private MyArrayList arrEmp, arrOne,arrInt, arrStr, arrFour;

    /**
     * Run before every test
     */
    @Before
    public void setUp(){
        arrEmp = new MyArrayList<String>(arrEmptyStr);
        arrOne = new MyArrayList<Integer>(arrSingle);
        arrInt = new MyArrayList<Integer>(arrInteger);
        arrStr = new MyArrayList<String>(arrString);
        arrFour = new MyArrayList<String>(listString);
    }


    /**
     * Tests reverseRegion method when either fromIndex or toIndex
     * or both are out of bounds.
     */
    @Test
    public void testReverseIndexOutOfBounds(){
        //TODO: Add your test case

        //check the correct behavior that in bounds
        arrStr.reverseRegion(0, 2);
        assertEquals("check reverse", "hello", arrStr.data[0]);
        assertEquals("check reverse", "hey", arrStr.data[2]);
        
        //check the incorrect behavior that out of bound
        boolean exceptionThrown = false;
        try {
            //int array out of bounds
            arrInt.reverseRegion(0, 4);
        } 
        catch (IndexOutOfBoundsException e) {
            exceptionThrown = true;
        }
        assertTrue("Check indexoutofbound", exceptionThrown); 
    }

    /**
     * Tests reverseRegion method when
     * fromIndex > toIndex
     */
    @Test
    public void testReverseFromIndexGreater(){
        //TODO: Add your test case

        //the fromIndex is greater than toIndex, and both in bounds
        arrInt.reverseRegion(3, 2);

        //check the list unchanged
        assertEquals("data not change", 1, arrInt.data[0]);
        assertEquals("data not change", 2, arrInt.data[1]);
        assertEquals("data not change", 2, arrInt.data[2]);
        assertEquals("data not change", 3, arrInt.data[3]);
        
        //check not throw an exception
        boolean exceptionThrown = false;
        try {
            //int array out of bounds
            arrInt.reverseRegion(3, 2);
        } 
        catch (IndexOutOfBoundsException e) {
            exceptionThrown = true;
        }
        assertFalse("Check indexoutofbound", exceptionThrown);
    }

    /**
     * Tests reverseRegion method when
     * fromIndex and toIndex are within bounds
    */
    @Test
    public void testReverseIndexWithinBounds(){
        //TODO: Add your test case
        //fromIndex < toIndex, both are in the middle of the list, in bounds 
        arrFour.reverseRegion(1, 2);
        assertEquals("not change", "hey", arrFour.data[0]);
        assertEquals("reverse", "hello", arrFour.data[1]);
        assertEquals("reverse", "hi", arrFour.data[2]);
        assertEquals("not change", "bye", arrFour.data[3]);
        }

    /**
     * Custom test
    */
    @Test
    public void testReverseCustom(){
        //TODO: Add your test case
    /**
    * test the reverse fromIndex = toIndex, in bounds, data not change
    * before only test fromIndex greater to toIndex, but not equal to toIndex
    * the result is the data won't check, and we can go and check
    */
        arrStr.reverseRegion(1, 1);
        assertEquals("data not change", "hey", arrStr.data[0]);
        assertEquals("data not change", "hi", arrStr.data[1]);
        assertEquals("data not change", "hello", arrStr.data[2]);
        //right now arrInt is {"hey","hello","hi"}, data unchange

        arrInt.reverseRegion(0, 0);
        assertEquals("data not change", 1, arrInt.data[0]);
        assertEquals("data not change", 2, arrInt.data[1]);
        assertEquals("data not change", 2, arrInt.data[2]);
        assertEquals("data not change", 3, arrInt.data[3]);
        //right now arrInt is {3,2,2,1}, data unchange

    }

    /**
     * Tests reverseRegion method with Empty String, 
     * or only one element String
    */
    @Test
    public void testStringReverseEmptyorSingle(){
        //empty arraylist
        boolean exceptionThrown1 = false;
        try {
            //int array out of bounds
            arrEmp.reverseRegion(0, 3);
        } 
        catch (IndexOutOfBoundsException e) {
            exceptionThrown1 = true;
        }
        assertTrue("Check indexoutofbound", exceptionThrown1);

        //arraylist only has one element
        boolean exceptionThrown2 = false;
        try {
            //int array out of bounds
            arrOne.reverseRegion(0, 1);
        } 
        catch (IndexOutOfBoundsException e) {
            exceptionThrown2 = true;
        }
        assertTrue("Check indexoutofbound", exceptionThrown2);
    }



    /**
     * Tests reverseRegion method randomly with String
     */
    @Test
    public void testStringReverseRandomly(){
        //reverse fromIndex < toIndex, in bounds
        arrStr.reverseRegion(1,2);
        assertEquals("data not change", "hey", arrStr.data[0]);
        assertEquals("reverse", "hello", arrStr.data[1]);
        assertEquals("reverse", "hi", arrStr.data[2]);
        //right now arrInt is {"hey","hello","hi"}, reversed

        //reverse fromIndex > toIndex, in bounds, data not change
        arrStr.reverseRegion(2, 0);
        assertEquals("data not change", "hey", arrStr.data[0]);
        assertEquals("data not change", "hello", arrStr.data[1]);
        assertEquals("data not change", "hi", arrStr.data[2]);
        //right now arrInt is {"hey","hello","hi"}, data unchange

        //reverse fromIndex > toIndex, out of bounds, data not change
        boolean exceptionThrown = false;
        try {
            //int array out of bounds
            arrStr.reverseRegion(3, 1);
        } 
        catch (IndexOutOfBoundsException e) {
            exceptionThrown = true;
        }
        assertTrue("Check indexoutofbound", exceptionThrown);
        //right now arrInt is {"hey","hello","hi"}, data unchange

        //reverse fromIndex < toIndex, in bounds
        arrStr.reverseRegion(0, 1);
        assertEquals("reverse", "hello", arrStr.data[0]);
        assertEquals("reverse", "hey", arrStr.data[1]);
        assertEquals("data not change", "hi", arrStr.data[2]);
        //right now arrInt is {"hello","hey","hi"}, reversed
    }


    /**
     * Tests reverseRegion method randomly with Interger
     */
    @Test
    public void testIntergerReverseRandomly(){
        //reverse fromIndex < toIndex, in bounds
        arrInt.reverseRegion(1,2);
        assertEquals("data not change", 1, arrInt.data[0]);
        assertEquals("reverse", 2, arrInt.data[1]);
        assertEquals("reverse", 2, arrInt.data[2]);
        assertEquals("data not change", 3, arrInt.data[3]);
        //right now arrInt is {1,2,2,3}, reversed

        //reverse fromIndex > toIndex, in bounds, data not change
        arrInt.reverseRegion(3, 0);
        assertEquals("data not change", 1, arrInt.data[0]);
        assertEquals("data not change", 2, arrInt.data[1]);
        assertEquals("data not change", 2, arrInt.data[2]);
        assertEquals("data not change", 3, arrInt.data[3]);
        //right now arrInt is {1,2,2,3}, data unchange

        //reverse fromIndex > toIndex, out of bounds, data not change
        boolean exceptionThrown = false;
        try {
            //int array out of bounds
            arrInt.reverseRegion(4, 1);
        } 
        catch (IndexOutOfBoundsException e) {
            exceptionThrown = true;
        }
        assertTrue("Check indexoutofbound", exceptionThrown);
        //right now arrInt is {1,2,2,3}, data unchange

        //reverse fromIndex < toIndex, in bounds
        arrInt.reverseRegion(0, 3);
        assertEquals("reverse", 3, arrInt.data[0]);
        assertEquals("data not change", 2, arrInt.data[1]);
        assertEquals("data not change", 2, arrInt.data[2]);
        assertEquals("reverse", 1, arrInt.data[3]);
        //right now arrInt is {3,2,2,1}, reversed
    }
}
```



