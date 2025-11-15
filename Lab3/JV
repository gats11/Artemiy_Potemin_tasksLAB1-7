import java.util.*;
/*

Реализация бинарной кучи

*/


// Simple implementation of a Min-Binary Heap in Java
public class binary<T extends Comparable<T>> {
    private final ArrayList<T> heap;

    public binary() {
        heap = new ArrayList<>(); // Using an array list to store the elements
    }

    // Insert operation: adds element at the bottom-rightmost position
    // Then moves it upwards as needed
    public void insert(T item) {
        heap.add(item);       // Add the new item at the end
        siftUp(heap.size() - 1); // Move it upward until heap property is satisfied
    }

    // Extract minimum element from the root
    public T extractMin() throws Exception {
        if (isEmpty()) throw new Exception("Heap is empty");

        T result = heap.get(0); // Get the root (minimum element)
        swap(0, heap.size() - 1); // Swap with the last element
        heap.remove(heap.size() - 1); // Remove the last element
        siftDown(0); // Restore heap property by sifting down
        return result;
    }

    // Check if heap is empty
    public boolean isEmpty() {
        return heap.isEmpty();
    }

    // Swap two items in the heap
    private void swap(int i, int j) {
        T temp = heap.get(i);
        heap.set(i, heap.get(j));
        heap.set(j, temp);
    }

    // Sift-up procedure for insertion
    private void siftUp(int idx) {
        while (idx > 0 && heap.get(parent(idx)).compareTo(heap.get(idx)) > 0) {
            swap(idx, parent(idx)); // If current element is smaller than its parent, swap them
            idx = parent(idx);     // Continue moving up
        }
    }

    // Sift-down procedure for extracting minimum
    private void siftDown(int idx) {
        while (!isLeaf(idx)) {
            int smallestChildIdx = getSmallestChildIndex(idx);
            if (smallestChildIdx != -1 &&
                    heap.get(smallestChildIdx).compareTo(heap.get(idx)) < 0) {
                swap(idx, smallestChildIdx); // Swap with the smallest child
                idx = smallestChildIdx;     // Move downward
            } else {
                break;                      // Stop when no further swaps are necessary
            }
        }
    }

    // Helper method to find the smallest child
    private int getSmallestChildIndex(int idx) {
        int leftChildIdx = leftChild(idx);
        int rightChildIdx = rightChild(idx);

        if (leftChildIdx >= heap.size()) return -1; // No children exist
        if (rightChildIdx >= heap.size()) return leftChildIdx; // Only one child exists

        return heap.get(leftChildIdx).compareTo(heap.get(rightChildIdx)) <= 0 ? leftChildIdx : rightChildIdx;
    }

    // Return true if given index represents a leaf node
    private boolean isLeaf(int idx) {
        return leftChild(idx) >= heap.size();
    }

    // Calculate parent's index
    private int parent(int idx) {
        return (idx - 1) / 2;
    }

    // Calculate left child's index
    private int leftChild(int idx) {
        return 2 * idx + 1;
    }

    // Calculate right child's index
    private int rightChild(int idx) {
        return 2 * idx + 2;
    }

    @Override
    public String toString() {
        return heap.toString();
    }

    public static void main(String[] args) throws Exception {
        binary<Integer> bh = new binary<>();
        bh.insert(10);
        bh.insert(5);
        bh.insert(8);
        bh.insert(3);
        System.out.println(bh.extractMin()); // Output should be 3
    }
}

/*


Реализация биномиальной кучи 


*/

class BinomialTreeNode<T extends Comparable<T>> {
    T data;
    BinomialTreeNode<T> parent;
    BinomialTreeNode<T> child;
    BinomialTreeNode<T> sibling;
    int degree; // Степень дерева

    public BinomialTreeNode(T data) {
        this.data = data;
        this.parent = null;
        this.child = null;
        this.sibling = null;
        this.degree = 0;
    }
}

// Основная структура биномиальной кучи
public class lab3<T extends Comparable<T>> {
    private BinomialTreeNode<T> head;

    // Создаем новую пустую кучу
    public lab3() {
        this.head = null;
    }

    // Создание одиночного дерева заданной степени
    private BinomialTreeNode<T> createSingleTree(BinomialTreeNode<T> x) {
        x.parent = null;
        x.child = null;
        x.sibling = null;
        x.degree = 0;
        return x;
    }

    // Вставляем новый элемент в кучу
    public void insert(T value) {
        BinomialTreeNode<T> newNode = new BinomialTreeNode<>(value);
        lab3<T> singleNodeHeap = new lab3<>();
        singleNodeHeap.head = createSingleTree(newNode);
        merge(singleNodeHeap); // объединяем с основной кучей
    }

    // Соединение двух деревьев одинаковой степени
    private BinomialTreeNode<T> linkTrees(BinomialTreeNode<T> y, BinomialTreeNode<T> z) {
        if (y.data.compareTo(z.data) > 0) {
            y.parent = z;
            z.child = y;
            return z;
        } else {
            z.parent = y;
            y.child = z;
            return y;
        }
    }

    // Объединение двух куч
    public void merge(lab3<T> other) {
        this.head = mergeHeaps(this.head, other.head);
    }

    // Внутреннее соединение двух кучи
    private BinomialTreeNode<T> mergeHeaps(BinomialTreeNode<T> firstHead, BinomialTreeNode<T> secondHead) {
        if (firstHead == null) return secondHead;
        if (secondHead == null) return firstHead;

        BinomialTreeNode<T> mergedHead = null;
        BinomialTreeNode<T> carry = null;
        BinomialTreeNode<T> tail = null;

        while (firstHead != null || secondHead != null || carry != null) {
            BinomialTreeNode<T> nextCarry = null;
            BinomialTreeNode<T> nextFirst = (firstHead != null) ? firstHead.sibling : null;
            BinomialTreeNode<T> nextSecond = (secondHead != null) ? secondHead.sibling : null;

            BinomialTreeNode<T> selectedNode = selectNext(firstHead, secondHead, carry);

            if (mergedHead == null) {
                mergedHead = selectedNode;
                tail = mergedHead;
            } else {
                tail.sibling = selectedNode;
                tail = tail.sibling;
            }

            firstHead = nextFirst;
            secondHead = nextSecond;
            carry = nextCarry;
        }

        return mergedHead;
    }

    // Выбор следующего дерева
    private BinomialTreeNode<T> selectNext(
            BinomialTreeNode<T> firstHead,
            BinomialTreeNode<T> secondHead,
            BinomialTreeNode<T> carry) {
        if (carry != null) {
            if ((firstHead == null || firstHead.degree > carry.degree) &&
                    (secondHead == null || secondHead.degree > carry.degree)) {
                return carry;
            }
        }

        if (firstHead != null && (secondHead == null || firstHead.degree < secondHead.degree))
            return firstHead;
        else
            return secondHead;
    }

    // Найти минимальный элемент
    public T findMinimum() {
        if (head == null) return null;
        BinomialTreeNode<T> curr = head;
        T minValue = curr.data;
        while (curr != null) {
            if (curr.data.compareTo(minValue) < 0) {
                minValue = curr.data;
            }
            curr = curr.sibling;
        }
        return minValue;
    }

    // Инвертирование списка (для корней орфанных детей)
    private BinomialTreeNode<T> reverseList(BinomialTreeNode<T> head) {
        BinomialTreeNode<T> prev = null;
        BinomialTreeNode<T> curr = head;
        while (curr != null) {
            BinomialTreeNode<T> next = curr.sibling;
            curr.sibling = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    // Проверка пустой кучи
    public boolean isEmpty() {
        return head == null;
    }

    // Преобразовать кучу в строку
    @Override
    public String toString() {
        return printTree(head);
    }

    // Рекурсивный вывод всех деревьев
    private String printTree(BinomialTreeNode<T> node) {
        if (node == null) return "";
        return node.data + "->" + printTree(node.sibling);
    }

    public static void main(String[] args) {
        lab3<Integer> heap = new lab3<>();
        heap.insert(10);
        heap.insert(5);
        heap.insert(8);
        heap.insert(3);
        System.out.println("Минимальный элемент: " + heap.findMinimum());
    }
}
/*

Реализация куч Фиббоначи

*/

class FibHeapNode<T extends Comparable<T>> {
    T key;
    FibHeapNode<T> parent, child, left, right;
    int degree;
    boolean marked;

    public FibHeapNode(T key) {
        this.key = key;
        this.parent = null;
        this.child = null;
        this.left = this;
        this.right = this;
        this.degree = 0;
        this.marked = false;
    }

    // Переопределение equals для правильного сравнения
    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof FibHeapNode)) return false;
        return ((FibHeapNode<?>) obj).key.equals(this.key);
    }

    // Связываем узлы друг с другом
    public void setSibling(FibHeapNode<T> sibling) {
        this.right = sibling;
        sibling.left = this;
    }

    // Разрыв кольца соседних узлов
    public void removeFromCircle() {
        this.left.right = this.right;
        this.right.left = this.left;
    }

    // Отрезание ребёнка от родителя
    public void cut() {
        if (this.parent != null) {
            this.parent.degree--;
            this.parent.child = this.right;
            this.parent = null;
        }
    }

    // Дополнительная маркировка
    public void cascadingCut() {
        if (this.parent != null) {
            if (!this.parent.marked) {
                this.parent.marked = true;
            } else {
                this.parent.cascadingCut();
            }
        }
    }
}

public class FibonacciHeap<T extends Comparable<T>> {
    private FibHeapNode<T> minNode;
    private int numNodes;

    public FibonacciHeap() {
        this.minNode = null;
        this.numNodes = 0;
    }

    // Добавление нового элемента
    public void insert(T key) {
        FibHeapNode<T> newNode = new FibHeapNode<>(key);
        if (minNode == null) {
            minNode = newNode;
        } else {
            minNode.setSibling(newNode);
            if (newNode.key.compareTo(minNode.key) < 0) {
                minNode = newNode;
            }
        }
        numNodes++;
    }

    // Нахождение минимального элемента
    public T findMin() {
        if (minNode == null) {
            throw new IllegalStateException("Heap is empty.");
        }
        return minNode.key;
    }

    // Удаление минимального элемента
    public T extractMin() {
        if (minNode == null) {
            throw new IllegalStateException("Heap is empty.");
        }

        T minKey = minNode.key;
        List<FibHeapNode<T>> roots = new LinkedList<>();

        // Все дети бывшего минимального элемента становятся новыми корнями
        FibHeapNode<T> child = minNode.child;
        while (child != null) {
            roots.add(child);
            child.parent = null;
            child = child.right;
        }

        // Удаляем старый минимум
        minNode.removeFromCircle();
        minNode = null;

        // Перераспределяем корневые деревья
        consolidate(roots);
        numNodes--;
        return minKey;
    }

    // Консолидация кучи (перегруппировка корней)
    private void consolidate(List<FibHeapNode<T>> roots) {
        int maxDegree = (int)(Math.log(numNodes) / Math.log((1 + Math.sqrt(5)) / 2)) + 1;
        FibHeapNode<T>[] degreeRoots = new FibHeapNode[maxDegree];

        for (FibHeapNode<T> root : roots) {
            int d = root.degree;
            while (degreeRoots[d] != null) {
                FibHeapNode<T> oldRoot = degreeRoots[d];
                if (root.key.compareTo(oldRoot.key) > 0) {
                    FibHeapNode<T> tmp = root;
                    root = oldRoot;
                    oldRoot = tmp;
                }
                link(root, oldRoot);
                degreeRoots[d++] = null;
            }
            degreeRoots[d] = root;
        }

        // Устанавливаем новый минимум
        minNode = null;
        for (FibHeapNode<T> root : degreeRoots) {
            if (root != null) {
                if (minNode == null || root.key.compareTo(minNode.key) < 0) {
                    minNode = root;
                }
            }
        }
    }

    // Присоединение одного дерева к другому
    private void link(FibHeapNode<T> parent, FibHeapNode<T> child) {
        child.removeFromCircle();
        child.parent = parent;
        child.marked = false;
        if (parent.child == null) {
            parent.child = child;
        } else {
            parent.child.setSibling(child);
        }
        parent.degree++;
    }

    // Количество элементов в куче
    public int size() {
        return numNodes;
    }

    // Печатаем содержимое кучи
    public void printHeap() {
        System.out.print("Current heap: ");
        FibHeapNode<T> curr = minNode;
        do {
            System.out.print(curr.key + ", ");
            curr = curr.right;
        } while (curr != minNode);
        System.out.println();
    }

    public static void main(String[] args) {
        FibonacciHeap<Integer> fibHeap = new FibonacciHeap<>();
        fibHeap.insert(10);
        fibHeap.insert(5);
        fibHeap.insert(8);
        fibHeap.insert(3);
        fibHeap.printHeap();
        System.out.println("Min element: " + fibHeap.findMin());
        fibHeap.extractMin();
        fibHeap.printHeap();
    }
}
/*

Реализация хэш-таблицы

*/
// Пара ключ-значение
class Entry<K, V> {
    K key;
    V value;

    public Entry(K key, V value) {
        this.key = key;
        this.value = value;
    }
}

// Основная хэш-таблица
public class HashTable<K, V> {
    private final List<List<Entry<K, V>>> buckets;
    private final int capacity;

    // Конструктор принимает начальную емкость
    public HashTable(int initialCapacity) {
        this.capacity = initialCapacity;
        this.buckets = new ArrayList<>(capacity);
        for (int i = 0; i < capacity; i++) {
            buckets.add(null);
        }
    }

    // Вставка пары ключ-значение
    public void put(K key, V value) {
        int bucketIndex = hashFunction(key.hashCode());
        List<Entry<K, V>> bucket = buckets.get(bucketIndex);
        if (bucket == null) {
            bucket = new LinkedList<>();
            buckets.set(bucketIndex, bucket);
        }
        for (Entry<K, V> entry : bucket) {
            if (entry.key.equals(key)) {
                entry.value = value; // Обновляем значение, если ключ уже существует
                return;
            }
        }
        bucket.add(new Entry<>(key, value)); // Добавляем новую пару
    }

    // Получение значения по ключу
    public V get(K key) {
        int bucketIndex = hashFunction(key.hashCode());
        List<Entry<K, V>> bucket = buckets.get(bucketIndex);
        if (bucket != null) {
            for (Entry<K, V> entry : bucket) {
                if (entry.key.equals(key)) {
                    return entry.value;
                }
            }
        }
        return null; // Значение не найдено
    }

    // Удаление элемента по ключу
    public void remove(K key) {
        int bucketIndex = hashFunction(key.hashCode());
        List<Entry<K, V>> bucket = buckets.get(bucketIndex);
        if (bucket != null) {
            for (Entry<K, V> entry : bucket) {
                if (entry.key.equals(key)) {
                    bucket.remove(entry);
                    return;
                }
            }
        }
    }

    // Хэш-функция для преобразования ключа в индекс
    private int hashFunction(int hashCode) {
        return Math.abs(hashCode % capacity);
    }

    // Размер таблицы
    public int size() {
        int count = 0;
        for (List<Entry<K, V>> bucket : buckets) {
            if (bucket != null) {
                count += bucket.size();
            }
        }
        return count;
    }

    // Показать содержимое таблицы
    public void display() {
        for (int i = 0; i < capacity; i++) {
            List<Entry<K, V>> bucket = buckets.get(i);
            if (bucket != null) {
                System.out.print("Bucket [" + i + "] -> ");
                for (Entry<K, V> entry : bucket) {
                    System.out.print("{" + entry.key + "=" + entry.value + "}, ");
                }
                System.out.println();
            }
        }
    }

    public static void main(String[] args) {
        HashTable<String, Integer> table = new HashTable<>(10);
        table.put("apple", 10);
        table.put("banana", 5);
        table.put("orange", 8);
        table.display();
        System.out.println(table.get("banana"));
        table.remove("apple");
        table.display();
    }
}
