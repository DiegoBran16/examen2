Problema No. 2
===================

 Clase ArrayList
 -----------------

~~~
package gt.edu.url.examen2.problema2;



import java.util.Iterator;
import java.util.NoSuchElementException;

/**
 *
 * @author diego
 * @param <E>
 */
public class ArrayList<E> implements List<E>{
	
	
        
	public static final int CAPACITY = 1;
	private E[] data;
	private int size = 0;
            /**
             * 
             * constructor 
             */
	public ArrayList() {
		this(CAPACITY);
	}
            /**
             * 
             * asigna la capacidad inicial del arreglo
             * @param capacity 
             */
	public ArrayList(int capacity) {
		data = (E[]) new Object[capacity];
	}
            /**
             * retorna la cantidad de elementos 
             * @return 
             */
	public int size() {
		return size;
	}
            /**
             * verifica si esta vacio
             * @return 
             */
	public boolean isEmpty() {
		return size == 0;
	}

	public E get(int i) {
		checkIndex(i, size);
		return data[i];
	}
            /**
             * cambia el valor deun  elemento en una pocsicion estaecida
             * @param i
             * @param e
             * @return 
             */
	public E set(int i, E e) {
		checkIndex(i, size);
		E temp = data[i];
		data[i] = e;
		return temp;
	}
            /**
             * 
             * añade un elemento en una posicion establecida
             * @param i
             * @param e 
             */
	public void add(int i, E e) {
		checkIndex(i, size + 1);
		if (size == data.length)
			resize(2 * data.length);
		for (int k = size - 1; k >= i; k--)
			data[k + 1] = data[k];
		data[i] = e; 
		size++;

	}
            /**
             * elimina un elemento del arreglo
             * @param i
             * @return
             * @throws IndexOutOfBoundsException 
             */
	public E remove(int i) throws IndexOutOfBoundsException {
		checkIndex(i, size);
		E temp = data[i];
		for (int k = i; k < size - 1; k++)
			data[k] = data[k + 1];
		data[size - 1] = null;
		size--;
		return temp;
	}
                /**
                 * verifica si el indice es valido 
                 * @param i
                 * @param n
                 * @throws IndexOutOfBoundsException 
                 */
	protected void checkIndex(int i, int n) throws IndexOutOfBoundsException {
		if (i < 0 || i >= n)
			throw new IndexOutOfBoundsException("Illegal index: " + i);
	}

	/**
	 * duplica el tamaño del arreglo al no tener posiciones disponibles
	 * @param capacity
	 */
	protected void resize(int capacity) {
		E[] temp = (E[]) new Object[capacity];
		for (int k=0; k < size; k++)
			temp[k] = data[k];
		data = temp;
	}

    @Override
    public Iterator<E> iterator() {
        throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
    }

}
~~~
interfaz DemoList
------------------

~~~
package gt.edu.url.examen2.problema2;

/**
 *
 * @author tuxtor
 */
public interface DemoList {
    List<Integer> crearDemoLista();
}
~~~
Clase DemostracionLista
------------------------
~~~
package gt.edu.url.examen2.problema2;

/**
 *
 * @author diego
 */
public class DemostracionLista implements DemoList{
        /**
         * 
         * @return retornq la lista con los elementos incertados
         */
     public List<Integer> crearDemoLista(){
         List<Integer> Lista= new ArrayList<>();
         Lista.add(0,4);
         Lista.add(0,3);
         Lista.add(0,2);
         Lista.add(2,1);
         Lista.add(1,5);
         Lista.add(1,6);
         Lista.add(3,7);
         Lista.add(0,8);
         return Lista;
     }
}
~~~
interfaz List
-----------------
~~~
package gt.edu.url.examen2.problema2;

public interface List<E> extends Iterable<E>{

	/**
	 * Returns list elements count
	 * @return number of elements in list
	 */
	int size();
	
	/**
	 * Checks if list is empty
	 * @return true if list is empty, false otherwise
	 */
	boolean isEmpty();

	/**
	 * Retrieves but not removes the element at index i
	 * @return Element at index i
	 */
	E get(int i);
	
	/**
	 * Replaces the element at index i with e, and returns the replaced element
	 * @return Replaced element at index i
	 */
	E set(int i, E e);
	
	
	/**
	 * Inserts element e to be at index i, shifting all subsequent elements later
	 * @param i Insertion point
	 * @param e Element to be inserted
	 */
	void add(int i, E e);
	
	/**
	 * Removes/returns the element at index i, shifting subsequent elements earlier
	 * @return Deleted element at index i
	 */
	 E remove(int i) throws IndexOutOfBoundsException;

}
~~~
Problema 3
=============
Clase LinkedPositionalList
--------------------------------
~~~
package gt.edu.url.examen2.problema3;

/**
 *
 * @author diego
 * @param <E>
 */
public class LinkedPositionalList<E> implements PositionalList<E>{

    /**
     * 
     * 
     * @param p
     * @param q 
     * recive 2 posiciones y las intercambia 
     */
    public void swap(Position<E> p, Position<E> q) {
       if (isEmpty()){
           System.out.println("esta vacia");
       }
       else{
           Position<E> tmp1= p;
            Position<E> tmp2=q;
            addBefore(tmp1,q.getElement());
            addBefore(tmp2,p.getElement());
            remove(p);
            remove(q);
            
           
           
       }
    }
    /**
     * 
     * nodo el cual recibe un elemento
     * @param <E> 
     */
    
   private static class Node<E> implements Position<E> {
		private E element;
		private Node<E> prev;// Anterior
		private Node<E> next;// Siguiente
                 /**
                  * 
                  * constructor del nodo
                  * @param e
                  * @param p
                  * @param n 
                  */
		public Node(E e, Node<E> p, Node<E> n) {
			element = e;
			prev = p;
			next = n;
		}
                /**
                 * obtiene el elemento del nodo 
                 * @return
                 * @throws IllegalStateException 
                 */

		public E getElement() throws IllegalStateException {
			if (next == null) // Nodo no valido
				throw new IllegalStateException("Posicion invalida");
			return element;
		}
                /**
                 * 
                 * asigna elemento al nodo
                 * @param e 
                 */
		
		public void setElement(E e) {
			element = e;
		}
                    /**
                     * 
                     * obtiene el nodo anterior 
                     * @return 
                     */
		public Node<E> getPrev() {
			return prev;
		}
                        /**
                         * asigna el nodo anterior
                         * @param prev 
                         */
		public void setPrev(Node<E> prev) {
			this.prev = prev;
		}
                /**
                 * 
                 * obtirne el nodo siguiente
                 * @return 
                 */

		public Node<E> getNext() {
			return next;
		}
                 /**
                  * asigna el nodo siguente 
                  * @param next 
                  */
		public void setNext(Node<E> next) {
			this.next = next;
		}

	}

	private Node<E> header = null;// Referencia
	private Node<E> trailer = null;
	private int size = 0;

	public LinkedPositionalList() {
		header = new Node<>(null, null, null);
		trailer = new Node<>(null, header, null);
		header.setNext(trailer);
	}

	// Metodos internos
	/**
	 * Valida si una posicion contiene un nodo y el nodo existe
	 */
	private Node<E> validate(Position<E> p) throws IllegalArgumentException {
		if (!(p instanceof Node))
			throw new IllegalArgumentException("P invalido");
		Node<E> node = (Node<E>) p; // safe cast
		if (node.getNext() == null)
			throw new IllegalArgumentException("p ya no se encuentra en la lista");
		return node;
	}
	
	/**
	 * "Empaca" un nodo como posicion a menos que sea header o trailer
	 */
	private Position<E> position(Node<E> node) {
		if (node == header || node == trailer)
			return null; // do not expose user to the sentinels
		return node;
	}

	// ---

	public int size() {return size;}

	public boolean isEmpty() {return size == 0;}

	public Position<E> first( ) {
		return position(header.getNext());
	}

	public Position<E> last( ) {
		return position(trailer.getPrev());
	}
	
	public Position<E> before(Position<E> p) throws IllegalArgumentException {
		Node<E> node = validate(p);
		return position(node.getPrev());
	}

	public Position<E> after(Position<E> p) throws IllegalArgumentException {
		Node<E> node = validate(p);
		return position(node.getNext());
	}
	
	
	private Position<E> addBetween(E e, Node<E> pred, Node<E> succ) {
           
		Node<E> newest = new Node<>(e, pred, succ);
		succ.setPrev(newest);
                pred.setNext(newest);
		size++;
		return newest;
	}
	
	public Position<E> addFirst(E e) {
		return addBetween(e, header, header.getNext());
	}

	public Position<E> addLast(E e) {
		return addBetween(e, trailer.getPrev(), trailer);
	}
	
	public Position<E> addBefore(Position<E> p, E e) throws IllegalArgumentException{
		Node<E> node = validate(p);
		return addBetween(e, node.getPrev(), node);
	}
	
	public Position<E> addAfter(Position<E> p, E e) throws IllegalArgumentException{ 
		Node<E> node = validate(p);
		return addBetween(e, node, node.getNext());
	}
              
	public E set(Position<E> p, E e) throws IllegalArgumentException {
		Node<E> node = validate(p);
		E answer = node.getElement();
		node.setElement(e);
		return answer;
	}
	
   @Override
        /**
         * 
         * elimina el elemento de la lista 
         */
	public E remove(Position<E> p) throws IllegalArgumentException {
		Node<E> node = validate(p);
		Node<E> predecessor = node.getPrev();
		Node<E> successor = node.getNext();
		predecessor.setNext(successor);
		successor.setPrev(predecessor);
		size--;
		E answer = node.getElement();
		node.setElement(null);
		node.setNext(null);
		node.setPrev(null);
		return answer;
	}
}

~~~

Interfaz Position
-----------------------
~~~
package gt.edu.url.examen2.problema3;

public interface Position<E> {
	
	E getElement() throws IllegalStateException;

}
~~~
interfaz PositionalList
----------------------------
~~~
package gt.edu.url.examen2.problema3;

public interface PositionalList<E> {
	int size( );
	boolean isEmpty( );
	Position<E> first( );
	Position<E> last( );
	Position<E> addFirst(E e);
	Position<E> addLast(E e);
	Position<E> before(Position<E> p) throws IllegalArgumentException;
	Position<E> after(Position<E> p) throws IllegalArgumentException;
	Position<E> addBefore(Position<E> p, E e) throws IllegalArgumentException;
	Position<E> addAfter(Position<E> p, E e) throws IllegalArgumentException;
	E set(Position<E> p, E e) throws IllegalArgumentException;
	E remove(Position<E> p) throws IllegalArgumentException;
        //Metodo a implementar
        void swap(Position<E> p, Position<E> q);

}
~~~

Problema 4
================
Interfaz Position
-----------------------
~~~
package gt.edu.url.examen2.problema3;

public interface Position<E> {
	
	E getElement() throws IllegalStateException;

}
~~~

interfaz PositionalList
---------------------------
~~~
package gt.edu.url.examen2.problema4;

public interface PositionalList<E> {
	int size( );
	boolean isEmpty( );
	Position<E> first( );
	Position<E> last( );
	Position<E> addFirst(E e);
	Position<E> addLast(E e);
	Position<E> before(Position<E> p) throws IllegalArgumentException;
	Position<E> after(Position<E> p) throws IllegalArgumentException;
	Position<E> addBefore(Position<E> p, E e) throws IllegalArgumentException;
	Position<E> addAfter(Position<E> p, E e) throws IllegalArgumentException;
	E set(Position<E> p, E e) throws IllegalArgumentException;
	E remove(Position<E> p) throws IllegalArgumentException;
        //Metodo a implementar
        Position<E> positionAtIndex(int i);
        

}
~~~
Clase PositionalLinkedList 
----------------------------------
~~~
package gt.edu.url.examen2.problema4;


/**
 *
 * @author diego
 * @param <E>
 */
public class LinkedPositionalList<E> implements PositionalList<E>{

    /**
     * 
     * 
     * @param i
     * @return retorna la pocicion en el indice i 
     */
    public Position<E> positionAtIndex(int i) {
        int px=0;
        
        Node<E> tmp= header.getNext();
       do{
           
           tmp=tmp.getNext();
           px++;
               
           }while(px!=i || tmp!=null);
       if(px==i){
           return tmp;
       }
       
           else{
       Exception IndexOutOfBoundsException = null;
        return (Position<E>) IndexOutOfBoundsException;
                  }
    }

    
   private static class Node<E> implements Position<E> {
		private E element;
		private Node<E> prev;// Anterior
		private Node<E> next;// Siguiente

		public Node(E e, Node<E> p, Node<E> n) {
			element = e;
			prev = p;
			next = n;
		}

		public E getElement() throws IllegalStateException {
			if (next == null) // Nodo no valido
				throw new IllegalStateException("Posicion invalida");
			return element;
		}
		
		public void setElement(E e) {
			element = e;
		}

		public Node<E> getPrev() {
			return prev;
		}

		public void setPrev(Node<E> prev) {
			this.prev = prev;
		}

		public Node<E> getNext() {
			return next;
		}

		public void setNext(Node<E> next) {
			this.next = next;
		}

	}

	private Node<E> header = null;// Referencia
	private Node<E> trailer = null;
	private int size = 0;

	public LinkedPositionalList() {
		header = new Node<>(null, null, null);
		trailer = new Node<>(null, header, null);
		header.setNext(trailer);
	}

	// Metodos internos
	/**
	 * Valida si una posicion contiene un nodo y el nodo existe
	 */
	private Node<E> validate(Position<E> p) throws IllegalArgumentException {
		if (!(p instanceof Node))
			throw new IllegalArgumentException("P invalido");
		Node<E> node = (Node<E>) p; // safe cast
		if (node.getNext() == null)
			throw new IllegalArgumentException("p ya no se encuentra en la lista");
		return node;
	}
	
	/**
	 * "Empaca" un nodo como posicion a menos que sea header o trailer
	 */
	private Position<E> position(Node<E> node) {
		if (node == header || node == trailer)
			return null; // do not expose user to the sentinels
		return node;
	}

	// ---

	public int size() {return size;}

	public boolean isEmpty() {return size == 0;}

	public Position<E> first( ) {
		return position(header.getNext());
	}

	public Position<E> last( ) {
		return position(trailer.getPrev());
	}
	
	public Position<E> before(Position<E> p) throws IllegalArgumentException {
		Node<E> node = validate(p);
		return position(node.getPrev());
	}

	public Position<E> after(Position<E> p) throws IllegalArgumentException {
		Node<E> node = validate(p);
		return position(node.getNext());
	}
	
	
	private Position<E> addBetween(E e, Node<E> pred, Node<E> succ) {
           
		Node<E> newest = new Node<>(e, pred, succ);
		succ.setPrev(newest);
                pred.setNext(newest);
		size++;
		return newest;
	}
	
	public Position<E> addFirst(E e) {
		return addBetween(e, header, header.getNext());
	}

	public Position<E> addLast(E e) {
		return addBetween(e, trailer.getPrev(), trailer);
	}
	
	public Position<E> addBefore(Position<E> p, E e) throws IllegalArgumentException{
		Node<E> node = validate(p);
		return addBetween(e, node.getPrev(), node);
	}
	
	public Position<E> addAfter(Position<E> p, E e) throws IllegalArgumentException{ 
		Node<E> node = validate(p);
		return addBetween(e, node, node.getNext());
	}

	public E set(Position<E> p, E e) throws IllegalArgumentException {
		Node<E> node = validate(p);
		E answer = node.getElement();
		node.setElement(e);
		return answer;
	}
	
   @Override
	public E remove(Position<E> p) throws IllegalArgumentException {
		Node<E> node = validate(p);
		Node<E> predecessor = node.getPrev();
		Node<E> successor = node.getNext();
		predecessor.setNext(successor);
		successor.setPrev(predecessor);
		size--;
		E answer = node.getElement();
		node.setElement(null);
		node.setNext(null);
		node.setPrev(null);
		return answer;
	}
}


~~~
Problema 5
===================
Interfaz Stack
----------------------
~~~
package gt.edu.url.examen2.problema5;

/**
 * Simple stack ADT representation
 * 
 * @author Víctor Orozco based on code from:
 ∗ @author Michael T. Goodrich
 ∗ @author Roberto Tamassia
 ∗ @author Michael H. Goldwasser
 */

public interface Stack<E> {

	/**
	 * Returns stack elements count
	 * @return number of elements in stack
	 */
	int size();
	
	/**
	 * Checks if stack is empty
	 * @return true if stack is empty, false otherwise
	 */
	boolean isEmpty();
	
	/**
	 * Inserts an element in the stack
	 * @param e element to be inserted
	 */
	void push(E e);
	
	/**
	 * Retrieves the last element of the stack without remotion
	 * @return Last stack element (null if empty)
	 */
	E top();
	
	/**
	 * Retrieves the last element of the stack removing it
	 * @return Last stack element (null if empty)
	 */
	E pop();
}
~~~

Clase DynamicStack
----------------------------
~~~
package gt.edu.url.examen2.problema5;

/**
 *
 * @author diego
 */
public class DynamicStack<E> implements Stack<E>{

	/**
	 * Inner class
	 * @author tuxtor
	 *
	 * @param <E>
	 */
	private static class Node<E>{
		private E element; //Valor
		private Node<E> next; //Puntero en la lista
		public Node(E element, Node<E> next) {
			super();
			this.element = element;
			this.next = next;
		}
		public E getElement() {
			return element;
		}
		public Node<E> getNext() {
			return next;
		}
		public void setNext(Node<E> next) {
			this.next = next;
		}
	}

	private Node<E> head = null;
	private Node<E> tail = null;
	
	private int size = 0;
	
	public int size() {return size;}
	
	public boolean isEmpty() { return size == 0;}
	
	private E first() {
		if (isEmpty()) return null;
		return head.getElement();
	}
	
	private E last() {
		if (isEmpty()) return null;
		return tail.getElement();
	}
	
	private void addFirst(E e) {
		head = new Node<>(e, head);
		if (size == 0) tail = head;
		size++;
	}
	
	private void addLast(E e) {
		Node<E> newest = new Node<>(e, null);
		if(isEmpty())
			head = newest;
		else
			tail.setNext(newest);
		tail = newest;
		size++;
	}
	
	private E removeFirst() {
		if (isEmpty()) return null;
		E response = head.getElement();
		head = head.getNext( );
		size--;
		if(size == 0) tail = null;
		return response;
	}

	@Override
	public void push(E e) {
		this.addFirst(e);
		
	}

	@Override
	public E top() {
		
		return this.first();
	}

	@Override
	public E pop() {
		
		return this.removeFirst();
	}


	
}

~~~




