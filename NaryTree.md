## Nary Tree

```
/**
 * TODO: Add file header
 * Name: Jiajia Li
 * ID: A17158450
 * Email: jil273@ucsd.edu
 * File description: This file contain the CSE12NaryTree class,
 * which E extends Comparable<E>. With the methods, it can add,
 * contains, sort and so on in list.
 */

import java.util.List;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;
import java.util.*;   

/**
 * TODO: Add class header
 * This class called CSE12NaryTree, and <E> extends Comparable<E>,
 * We can use the methods to add, contains, sort the element.
 */
public class CSE12NaryTree<E extends Comparable<E>> {
    
    /**
     * This inner class encapsulates the data and children for a Node.
     * Do NOT edit this inner class.
     */
    protected class Node{
        E data;
        List<Node> children;
    
        /**
         * Initializes the node with the data passed in
         * 
         * @param data The data to initialize the node with
         */
        public Node(E data) {
            this.data = data;
            this.children = new ArrayList<>();
        }
    
        /**
         * Getter for data
         * 
         * @return Return a reference to data
         */
        public E getData() {
            return data;
        }

        /**
         * Setter for the data
         * 
         * @param data Data that this node is set to
         */
        public void setData(E data) {
            this.data = data;
        }

        /**
         * Getter for children
         * 
         * @return reference to the list of children
         */
        public List<Node> getChildren() {
            return children;
        }

        /**
         * Returns the number of children
         * 
         * @return number of children
         */
        public int getNumChildren() {
            // assume there are no nulls in list
            return children.size();
        }

        /**
         * Add the given node to this node's list of children
         * 
         * @param node The node to add
         */
        public void addChild(Node node) {
            children.add(node);
        }
    
    }
    
    Node root;
    int size;
    int N;

    /**
     * Constructor that initializes an empty N-ary tree, with the given N
     * 
     * @param N The N the N-tree should be initialized with
     */
    public CSE12NaryTree(int N) {
        if (N <= 0) {
            throw new IllegalArgumentException();
        }
        this.root = null;
        this.size = 0;
        this.N = N;
    }

    /**
     * TODO: Add method header
     * 
     * Add a new Node containing element to the N-ary tree in 
     * level order and update size accordingly.
     * 
     * @param element the element that needs to add to the N-ary tree
     */
    public void add(E element) {
        //TODO
        if(element == null){
            throw new NullPointerException();
        }
        Node newNode = new Node(element); // the element to be added
        if(root == null){
            root = newNode;
            size = size + 1;
            return;
        }else{
            // create a queue to perform level order traversal
            Queue<Node> q = new LinkedList<Node>();
            q.add(root);
            // keep looking for a node that does not have 
            //full children list
            while(!q.isEmpty()){
                Node nextNode = q.remove();
                if(nextNode.getNumChildren() < N){
                    // position found, insert and return
                    nextNode.addChild(newNode);
                    size = size + 1;
                    return;
                }
                // position not found, add all children of 
                //current node to the queue
                for(Node child : nextNode.getChildren()){
                    q.add(child);
                }
            }
        }
    }

    /**
     * TODO: Add method header
     * 
     * Check if element is in the N-ary tree.
     * 
     * @param element the element that we need to check
     * @return true if element is in the N-ary tree or false otherwise
     */
    public boolean contains(E element) {
        //TODO
        if(element == null){
            throw new NullPointerException();
        }
        if(root == null){
            return false;
        //if(root !- root)
        }else{
            ArrayList<E> sorted = sortTree();
            for(int i = 0; i < sorted.size(); i++){
                if(sorted.get(i) == element){
                    return true;
                }
            }
        }
        return false;
    }

    /**
     * TODO: Add method header
     * 
     * Use a PriorityQueue and perform heap sort on the tree and return 
     * an ArrayList of all elements in the tree in sorted order
     * 
     * @return an ArrayList of all elements in the tree in sorted order
     */
    public ArrayList<E> sortTree(){
        //TODO

        ArrayList<E> sorted = new ArrayList<E>();
        if(root == null){
            return sorted;
        }else{
            // create a queue to perform level order traversal
            Queue<Node> q = new LinkedList<Node>();
            q.add(root);
            // keep looking for a node that does not have 
            //full children list
            while(!q.isEmpty()){
                Node nextNode = q.remove();
                sorted.add(nextNode.getData());
                // position not found, add all children of 
                //current node to the queue
                for(Node child : nextNode.getChildren()){
                    q.add(child);
                }
            }
        }
        Collections.sort(sorted);
        return sorted;
    }
}

```

## Nary Tree Test

```
/**
 * TODO: Add file header
 * Name: Jiajia Li
 * ID: A17158450
 * Email: jil273@ucsd.edu
 * File description: This file contains the tests class for 
 * CSE12NaryTree.java, we can use the tests to check if the 
 * methods in CSE12NaryTree.java work correctly.
 */
 
import static org.junit.Assert.*;
import org.junit.*;
import java.util.*;

/**
 * TODO: Add class header
 * This class contains the tests for CSE12NaryTree, we can use 
 * these tests to check if the methods, like add, contains, sort
 * in CSE12NaryTree.class work and with correct output.
 */
public class CSE12NaryTreeTester {

    /**
     * TODO: Add test case description.
     * To test add method on a 5-ary tree that contains a root 
     * and 5 children root.
     */
    @Test
    public void testAdd(){
        //set up
        CSE12NaryTree<Integer> tree = new CSE12NaryTree<>(5);
        tree.add(1);
        tree.add(2);
        tree.add(4);
        tree.add(4);
        tree.add(3);
        tree.add(6);

        tree.add(7);

        //check root
        assertEquals((Integer)1, tree.root.getData());
        //check children
        Integer[] expected = {2,4,4,3,6};
        List<CSE12NaryTree<Integer>.Node> list = tree.root.getChildren();
        for(int i = 0; i < 5; i++){
            assertEquals(expected[i], list.get(i).getData());   
        }
        //check first child's child
        assertEquals((Integer)7, 
            tree.root.getChildren().get(0).getChildren().get(0).getData());
        //check size
        assertEquals(7, tree.size);
    }

    /**
     * TODO: Add test case description
     * To test the contains method on a 5-ary tree when the 
     * element is not present in.
     */
    @Test
    public void testContains(){
        //set up
        CSE12NaryTree<Integer> tree = new CSE12NaryTree<>(5);
        tree.add(1);
        tree.add(2);
        tree.add(4);
        tree.add(4);
        tree.add(3);
        tree.add(6);

        //elements not present in
        assertFalse(tree.contains(7));
        assertFalse(tree.contains(8));
        assertFalse(tree.contains(5));
    }

    /**
     * TODO: Add test case description
     * To test sortTree method on a 5-ary tree that only contains
     * the root node.
     */
    @Test
    public void testSortTree(){
        //set up
        CSE12NaryTree<Integer> tree = new CSE12NaryTree<>(5);
        tree.add(3);

        ArrayList<Integer> expected = new ArrayList<Integer>();
        expected.add(3);
        
        //check sorted
        assertEquals(expected, tree.sortTree());
        //check size
        assertEquals(1, tree.size);
    }

    /**
     * TODO: Add test case description
     * To test the add method on a 5-ary tree that to add
     * a null element. Didn't test it before.
     */
    @Test
    public void testCustom(){
        //set up
        CSE12NaryTree<Integer> tree = new CSE12NaryTree<>(5);
        tree.add(1);
        tree.add(2);
        tree.add(4);
        tree.add(4);
        tree.add(3);
        tree.add(6);

        //check when add a null element, throw exception
        boolean exceptionThrown = false;
        try {
            tree.add(null);
        } 
        catch (NullPointerException e) {
            exceptionThrown = true;
        }
        assertTrue("Check NullPointerException", exceptionThrown);
        //check size unchanged
        assertEquals(6, tree.size);
    }

    /**
     * Extra test to test the add method on a 3-ary three that 
     * contains a root and 7 children nodes.
     */
    @Test
    public void testAddExtra(){
        //set up
        CSE12NaryTree<Integer> tree = new CSE12NaryTree<>(3);
        tree.add(3);
        tree.add(2);
        tree.add(4);
        tree.add(4);
        tree.add(3);
        tree.add(6);
        tree.add(8);
        tree.add(7);

        tree.add(9);

        //check root
        assertEquals((Integer)3, tree.root.getData());
        //check root's children
        Integer[] expectedone = {2,4,4};
        List<CSE12NaryTree<Integer>.Node> list1 = tree.root.getChildren();
        for(int i = 0; i < 3; i++){
            assertEquals(expectedone[i], list1.get(i).getData());   
        }
        //check root's children's first children
        Integer[] expectedtwo = {3,6,8};
        List<CSE12NaryTree<Integer>.Node> list2 = 
            tree.root.getChildren().get(0).getChildren();
        for(int i = 0; i < 3; i++){
            assertEquals(expectedtwo[i], list2.get(i).getData());   
        }
        //check root's children's second children
        Integer[] expectedthree = {7,9};
        List<CSE12NaryTree<Integer>.Node> list3 = 
            tree.root.getChildren().get(1).getChildren();
        for(int i = 0; i < 2; i++){
            assertEquals(expectedthree[i], list3.get(i).getData());   
        }
        //check size
        assertEquals(9, tree.size);
      }

    /**
     * Extra test to test contains on a 5-ary tree contains 
     * a root node and 5 children nodes.
     */
    @Test
    public void testContainsExtra(){
        //set up
        CSE12NaryTree<Integer> tree = new CSE12NaryTree<>(5);
        tree.add(1);
        tree.add(2);
        tree.add(4);
        tree.add(4);
        tree.add(3);
        tree.add(6);

        //check if contains
        assertTrue(tree.contains(1));
        assertTrue(tree.contains(2));
        assertTrue(tree.contains(4));
        assertTrue(tree.contains(3));
        assertFalse(tree.contains(0));
        assertFalse(tree.contains(9));

        //check if use contains null element, throw exception
        boolean exceptionThrown = false;
        try {
            tree.contains(null);
        } 
        catch (NullPointerException e) {
            exceptionThrown = true;
        }
        assertTrue("Check NullPointerException", exceptionThrown); 
    }

    /**
     * Extra test to test sortTree on a 5-ary tree contains 
     * a root node and 6 children nodes.
     */
    @Test
    public void testSortTreeExtra(){
        //set up
        CSE12NaryTree<Integer> tree = new CSE12NaryTree<>(5);
        tree.add(3);
        tree.add(2);
        tree.add(4);
        tree.add(4);
        tree.add(1);
        tree.add(6);
        tree.add(3);

        ArrayList<Integer> expected = new ArrayList<Integer>();
        expected.add(1);
        expected.add(2);
        expected.add(3);
        expected.add(3);
        expected.add(4);
        expected.add(4);
        expected.add(6);
        
        //check sort correctly
        assertEquals(expected, tree.sortTree());
        //check size
        assertEquals(7, tree.size);
    }
    
}
```
