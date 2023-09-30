## LinkedList
```
/**
 * TODO: Add your file header
 * Name: Jiajia Li
 * ID: A17158450
 * Email: jil273@ucsd.edu
 * File description: This file is inclue the methods and setting about 
 * linkedlist, and we need to add a method about reverseregion for it.
 */

/**
 * TODO: Add class header
 * This class is to set up the basic setting and method for the list to 
 * show like the linkedlist. Also, it included the method about reverse.
 */
public class MyLinkedList<E> implements MyReverseList<E>{

    int size;
    Node head;
    Node tail;

    /**
     * A Node class that holds data and references to previous and next Nodes
     * This class is used for both MyLinkedList and MyListIterator.
     */
    protected class Node {
        E data;
        Node next;
        Node prev;

        /** 
         * Constructor to create singleton Node 
         * @param element Element to add, can be null	
         */
        public Node(E element) {
            //Initialise the elements
            this.data = element;
            this.next = null;
            this.prev = null;
        }

        /** 
         * Set the previous node in the list
         * @param p new previous node
         */
        public void setPrev(Node p) {
            //Set the node p on the previous position
            prev = p;
        }

        /** 
         * Set the next node in the list
         * @param n new next node
         */
        public void setNext(Node n) {
            //Set the node n on the next position
            next = n;
        }

        /** 
         * Set the element 
         * @param e new element 
         */
        public void setElement(E e) {
            this.data = e;
        }

        /** 
         * Accessor to get the next Node in the list 
         * @return the next node
         */
        public Node getNext() {
            return this.next;
        }
        /** 
         * Accessor to get the prev Node in the list
         * @return the previous node  
         */
        public Node getPrev() {
            return this.prev;
        } 
        /** 
         * Accessor to get the Nodes Element 
         * @return this node's data
         */
        public E getElement() {
            return this.data;
        } 
    }

    //IMPORTANT: DO NOT MODIFY THIS CONSTRUCTOR!
    //IMPORTANT: DO NOT ADD ANY MORE CONSTRUCTORS!
    /**
     * Constructor to create a doubly linked list 
     * with the argument array's elements
     * @param arr - array of elements to be used to construct the LinkedList
     */
    public MyLinkedList(E[] arr) {

        //Create dummy nodes
        head = new Node(null);
        tail = new Node(null);
        head.setNext(tail);
        tail.setPrev(head);
        size = 0;	

        if(arr != null){
            //create list by inserting each element
            Node currNode = head;
            for(int i=0; i<arr.length; i++){
                Node newNode = new Node(arr[i]);
                currNode.next.prev = newNode;
                newNode.next = currNode.next;
                newNode.prev = currNode;
                currNode.next = newNode;

                //move pointer to the next node
                currNode = currNode.next;
                //increase size of list
                this.size++;
            }
        }
    }


    /**
     * TODO: Method header comment here
     * The method the reverse the elements in the list 
     * between fromIndex and toIndex.
     * @param fromIndex - start index of element that going to reverse
     * @param toIndex - end index of element that going to reverse
     */
    public void reverseRegion(int fromIndex, int toIndex){
        //TODO: Add your solution here
        if(fromIndex > size - 1 || toIndex > size - 1 ||
         fromIndex < 0 || toIndex < 0){
            throw new IndexOutOfBoundsException();
        }
        if(fromIndex < toIndex){
            for(int i = fromIndex; i < toIndex; i++){
                //get the element of fromIndex and toIndex
                E fromIndexele = get(fromIndex);
                E toIndexele = get(toIndex);
                //get the position of fromIndex and toIndex
                Node fromIndexNth = getNth(fromIndex);
                Node toIndexNth = getNth(toIndex);
                //get the temp node to store the element of them
                Node tempfrom = new Node(fromIndexele);
                Node tempto = new Node(toIndexele);
                //chenge the toIndex element to fromIndex
                fromIndexNth.prev.setNext(tempto);
                fromIndexNth.next.setPrev(tempto);
                tempto.setNext(fromIndexNth.next);
                tempto.setPrev(fromIndexNth.prev);
                //chenge the fromIndex element to toIndex
                toIndexNth.prev.setNext(tempfrom);
                toIndexNth.next.setPrev(tempfrom);
                tempfrom.setNext(toIndexNth.next);
                tempfrom.setPrev(toIndexNth.prev);
                ////change position of toIndex to reverse with different i
                toIndex--;
            }
        }
    }

    @Override
    /** 
     * Returns the number of elements stored
     * @return - number of elements in the linkedlist
    */
    public int size() {
        //Return the number of nodes in the linkedlist
        return this.size;
    }

    @Override
    /** 
     * Get contents at position i
     * @param index - The index position to obtain the data
     * @return the Element at the specified index
     */
    public E get(int index)	{

        Node currNode = this.getNth(index);

        //Get the value of data at the position
        E element = currNode.getElement();

        return element;	
    }


    /** A method that returns the node at a specified index,
     *  not the content
     *  @param index - position to return the node
     * @return - Node at 'index'
     */
    // Helper method to get the Node at the Nth index
    protected Node getNth(int index) {
        if (index >= this.size || index < 0)
            throw new IndexOutOfBoundsException();

        Node currNode = this.head;

        //Loop through the linked list and stop at the position
        for (int i = 0; i <= index; i++) {
            currNode = currNode.getNext();
        }

        //return the node	
        return currNode; 
    }

}
```

## LinkedList Reverse
```
/**
 * This file contains the MyReverseList interface
 * that defines the methods to be implemented 
 * by MyArrayList and MyLinkedList classes. 
 */

//IMPORTANT: DO NOT MODIFY THIS INTERFACE

/**
 * An interface that contains the methods used to 
 * reverse a given region in a list.
 */
public interface MyReverseList<E> {

    /* Get the number of elements in the list */
    int size();

    /* Reverses the given region in a list*/
    void reverseRegion(int fromIndex, int toIndex);

    /* Gets an element at specified index */
    E get(int index);

}
```
