#include <iostream>
#include <list>
#include <algorithm>


/*

Реализация кучи Фибоначчи

*/
template<typename T>
class FibonacciHeapNode {
public:
    T key;
    FibonacciHeapNode<T>* parent;
    FibonacciHeapNode<T>* child;
    FibonacciHeapNode<T>* left;
    FibonacciHeapNode<T>* right;
    unsigned int degree;
    bool mark;

    FibonacciHeapNode(const T& key) :
        key(key),
        parent(nullptr),
        child(nullptr),
        left(this),
        right(this),
        degree(0),
        mark(false) {}
};

template<typename T>
class FibonacciHeap {
private:
    FibonacciHeapNode<T>* min_node;
    unsigned int node_count;

public:
    FibonacciHeap() : min_node(nullptr), node_count(0) {}

    ~FibonacciHeap() {
        clear();
    }

    void insert(const T& key) {
        auto new_node = new FibonacciHeapNode<T>(key);
        if (min_node == nullptr) {
            min_node = new_node;
        } else {
            concatenate_with_root_list(new_node);
            if (new_node->key < min_node->key) {
                min_node = new_node;
            }
        }
        ++node_count;
    }

    const T& extract_min() {
        if (min_node == nullptr) {
            throw std::runtime_error("Heap is empty!");
        }

        auto extracted_node = min_node;
        if (extracted_node->child != nullptr) {
            // Переносим детей извлечённого узла в корневую зону
            FibonacciHeapNode<T>* child = extracted_node->child;
            do {
                auto next_child = child->right;
                child->parent = nullptr;
                concatenate_with_root_list(child);
                child = next_child;
            } while (child != extracted_node->child);
        }

        // Удаляем извлечённый узел из корневой зоны
        unlink_from_root_list(extracted_node);

        if (min_node == extracted_node) {
            if (extracted_node->right == extracted_node) {
                min_node = nullptr;
            } else {
                min_node = extracted_node->right;
                consolidate();
            }
        }

        delete extracted_node;
        --node_count;
        return min_node->key;
    }

    void decrease_key(FibonacciHeapNode<T>* node, const T& new_key) {
        if (new_key > node->key) {
            throw std::invalid_argument("The new key must be less than the current key!");
        }

        node->key = new_key;
        FibonacciHeapNode<T>* parent = node->parent;
        if (parent != nullptr && node->key < parent->key) {
            cut_and_cascade(node, parent);
        }

        if (node->key < min_node->key) {
            min_node = node;
        }
    }

    void clear() {
        while (min_node != nullptr) {
            extract_min();
        }
    }

    bool empty() const {
        return min_node == nullptr;
    }

    unsigned int size() const {
        return node_count;
    }

private:
    void concatenate_with_root_list(FibonacciHeapNode<T>* node) {
        if (min_node == nullptr) {
            min_node = node;
        } else {
            node->right = min_node->right;
            node->left = min_node;
            min_node->right->left = node;
            min_node->right = node;
        }
    }

    void unlink_from_root_list(FibonacciHeapNode<T>* node) {
        if (node->right == node) {
            min_node = nullptr;
        } else {
            node->right->left = node->left;
            node->left->right = node->right;
            if (min_node == node) {
                min_node = node->right;
            }
        }
    }

    void cut_and_cascade(FibonacciHeapNode<T>* node, FibonacciHeapNode<T>* parent) {
        // Отделяем узел от родителя
        node->left->right = node->right;
        node->right->left = node->left;
        parent->degree--;
        if (parent->child == node) {
            parent->child = node->right;
        }
        node->parent = nullptr;
        node->mark = false;
        concatenate_with_root_list(node);

        // Производим каскадное отсечение
        if (parent->mark) {
            cut_and_cascade(parent, parent->parent);
        } else if (parent->parent != nullptr) {
            parent->mark = true;
        }
    }

    void consolidate() {
        std::list<FibonacciHeapNode<T>*> root_list;
        root_list.splice(root_list.begin(), collect_root_list());

        // Сортируем деревья по рангу
        std::sort(root_list.begin(), root_list.end(),
                 [](const FibonacciHeapNode<T>* a, const FibonacciHeapNode<T>* b) {
                     return a->degree < b->degree;
                 });

        // Слияние деревьев одинакового ранга
        std::list<FibonacciHeapNode<T>*> merged_trees;
        for (auto iter = root_list.begin(); iter != root_list.end(); ) {
            auto cur = *iter;
            iter++;
            while (iter != root_list.end() && (*iter)->degree == cur->degree) {
                auto next = *iter;
                iter++;
                if (cur->key > next->key) {
                    std::swap(cur, next);
                }
                link_trees(cur, next);
            }
            merged_trees.push_back(cur);
        }

        // Устанавливаем новый минимальный узел
        min_node = nullptr;
        for (auto node : merged_trees) {
            if (min_node == nullptr || node->key < min_node->key) {
                min_node = node;
            }
        }
    }

    std::list<FibonacciHeapNode<T>*> collect_root_list() {
        std::list<FibonacciHeapNode<T>*> collected;
        if (min_node != nullptr) {
            FibonacciHeapNode<T>* start = min_node;
            do {
                collected.push_back(start);
                start = start->right;
            } while (start != min_node);
        }
        return collected;
    }

    void link_trees(FibonacciHeapNode<T>* y, FibonacciHeapNode<T>* x) {
        y->left->right = y->right;
        y->right->left = y->left;
        y->parent = x;
        y->mark = false;
        if (x->child == nullptr) {
            x->child = y;
        } else {
            y->right = x->child->right;
            y->left = x->child;
            x->child->right->left = y;
            x->child->right = y;
        }
        x->degree++;
    }
};

int main() {
    FibonacciHeap<int> fh;
    fh.insert(10);
    fh.insert(5);
    fh.insert(8);
    fh.insert(3);

    std::cout << "Extracted min: " << fh.extract_min() << "\n";
    return 0;
}
/*

Реализация хэш-таблицы

*/
#include <iostream>
#include <vector>
#include <list>
#include <string>

// Пара ключ-значение
template<typename K, typename V>
struct KVPair {
    K key;
    V value;
    KVPair(K k, V v) : key(k), value(v) {}
};

// Хэш-таблица с цепочками
template<typename K, typename V>
class HashTable {
private:
    std::vector<std::list<KVPair<K, V>>> table;
    size_t size;

    // Стандартная хэш-функция (можно заменить собственной функцией)
    size_t hash(const K& key) const {
        return std::hash<K>{}(key) % size;
    }

public:
    // Конструктор с начальной ёмкостью
    explicit HashTable(size_t init_size = 10) : size(init_size), table(init_size) {}

    // Вставка пары ключ-значение
    void insert(const K& key, const V& value) {
        size_t index = hash(key);
        auto& cell = table[index];  // Получаем список по индексу
        for (auto& pair : cell) {
            if (pair.key == key) {  // Если ключ уже существует, обновляем значение
                pair.value = value;
                return;
            }
        }
        cell.emplace_back(key, value);  // Иначе добавляем новую пару
    }

    // Поиск значения по ключу
    V* find(const K& key) {
        size_t index = hash(key);
        auto& cell = table[index];
        for (auto& pair : cell) {
            if (pair.key == key) {
                return &pair.value;  // Вернуть указатель на значение
            }
        }
        return nullptr;  // Если ключ не найден
    }

    // Удаление элемента по ключу
    void erase(const K& key) {
        size_t index = hash(key);
        auto& cell = table[index];
        for (auto it = cell.begin(); it != cell.end(); ++it) {
            if ((*it).key == key) {
                cell.erase(it);  // Удаляем найденную пару
                return;
            }
        }
    }

    // Печать содержимого таблицы
    void print() const {
        for (size_t i = 0; i < size; ++i) {
            std::cout << "Cell " << i << ": ";
            for (const auto& pair : table[i]) {
                std::cout << "(" << pair.key << ", " << pair.value << ") ";
            }
            std::cout << std::endl;
        }
    }
};

int main() {
    HashTable<std::string, int> ht(10);
    ht.insert("apple", 10);
    ht.insert("banana", 5);
    ht.insert("orange", 8);

    std::cout << "Apple value: " << *(ht.find("apple")) << std::endl;
    ht.erase("banana");
    ht.print();
    return 0;
}
/*

Реализация бинарной кучи

*/
#include <iostream>
#include <vector>

class BinaryHeap {
private:
    std::vector<int> heap;  // Хранение элементов кучи в векторном массиве

    // Вспомогательная функция для смены местами двух элементов
    void swap(int i, int j) {
        std::swap(heap[i], heap[j]);
    }

    // Просеивание вверх (для вставки)
    void sift_up(int index) {
        while (index > 0) {
            int parent_idx = (index - 1) / 2;
            if (heap[index] < heap[parent_idx]) {
                swap(index, parent_idx);
                index = parent_idx;
            } else {
                break;
            }
        }
    }

    // Просеивание вниз (для извлечения минимального элемента)
    void sift_down(int index) {
        int left_child_idx = 2 * index + 1;
        int right_child_idx = 2 * index + 2;
        int smallest = index;

        if (left_child_idx < heap.size() && heap[left_child_idx] < heap[smallest])
            smallest = left_child_idx;

        if (right_child_idx < heap.size() && heap[right_child_idx] < heap[smallest])
            smallest = right_child_idx;

        if (smallest != index) {
            swap(index, smallest);
            sift_down(smallest);  // Продолжаем рекурсию вниз
        }
    }

public:
    // Вставка элемента в кучу
    void insert(int value) {
        heap.push_back(value);  // Добавляем элемент в конец
        sift_up(heap.size() - 1);  // Восстанавливаем структуру кучи
    }

    // Извлечение минимального элемента
    int extract_min() {
        if (heap.empty()) {
            throw std::out_of_range("Heap is empty");
        }

        int min_val = heap.front();  // Берём корень (минимальный элемент)
        heap.front() = heap.back();  // Ставим последний элемент на место корня
        heap.pop_back();  // Удаляем последний элемент
        sift_down(0);  // Восстанавливаем структуру кучи
        return min_val;
    }

    // Возвращает минимальный элемент без удаления
    int peek_min() const {
        if (heap.empty()) {
            throw std::out_of_range("Heap is empty");
        }
        return heap.front();
    }

    // Размер кучи
    size_t size() const {
        return heap.size();
    }

    // Проверка пустоты
    bool empty() const {
        return heap.empty();
    }

    // Вывод кучи на экран
    friend std::ostream& operator<<(std::ostream& os, const BinaryHeap& heap) {
        os << "{ ";
        for (int elem : heap.heap) {
            os << elem << ' ';
        }
        os << "}";
        return os;
    }
};

int main() {
    BinaryHeap heap;
    heap.insert(10);
    heap.insert(5);
    heap.insert(8);
    heap.insert(3);

    std::cout << "Initial heap: " << heap << std::endl;
    std::cout << "Peek min: " << heap.peek_min() << std::endl;
    std::cout << "Extract min: " << heap.extract_min() << std::endl;
    std::cout << "Final heap: " << heap << std::endl;

    return 0;
}

/*

Реализация биномиальной кучи

*/
#include <iostream>
#include <vector>

// Структура узла биномиального дерева
struct BinomialTreeNode {
    int key;
    BinomialTreeNode* parent;
    BinomialTreeNode* child;
    BinomialTreeNode* sibling;
    int degree;  // Степень дерева

    BinomialTreeNode(int k) : key(k), parent(nullptr), child(nullptr), sibling(nullptr), degree(0) {}
};

// Биномиальная куча
class BinomialHeap {
private:
    BinomialTreeNode* head;  // Головной узел списка деревьев

    // Слияние двух деревьев одинаковой степени
    BinomialTreeNode* mergeTrees(BinomialTreeNode* t1, BinomialTreeNode* t2) {
        if (t1->key > t2->key) std::swap(t1, t2);
        t2->parent = t1;
        t2->sibling = t1->child;
        t1->child = t2;
        t1->degree++;
        return t1;
    }

    // Обход всех деревьев
    void traverseTrees(BinomialTreeNode* node) {
        while (node != nullptr) {
            std::cout << "Tree Degree: " << node->degree << ", Key: " << node->key << std::endl;
            node = node->sibling;
        }
    }

public:
    BinomialHeap() : head(nullptr) {}

    // Вставка нового элемента
    void insert(int key) {
        BinomialHeap singleHeap;
        singleHeap.head = new BinomialTreeNode(key);
        merge(*this, singleHeap);  // Совмещаем одиночное дерево с главной кучей
    }

    // Слияние двух биномиальных куч
    void merge(BinomialHeap& heap1, BinomialHeap& heap2) {
        head = mergeLists(heap1.head, heap2.head);
        consolidate();  // Консолидация (переформатирование кучи)
    }

    // Обработка списка деревьев после слияния
    BinomialTreeNode* mergeLists(BinomialTreeNode* l1, BinomialTreeNode* l2) {
        if (l1 == nullptr) return l2;
        if (l2 == nullptr) return l1;

        BinomialTreeNode* merged = nullptr;
        BinomialTreeNode* last = nullptr;
        while (l1 != nullptr && l2 != nullptr) {
            if (l1->degree <= l2->degree) {
                append(&last, &merged, l1);
                l1 = l1->sibling;
            } else {
                append(&last, &merged, l2);
                l2 = l2->sibling;
            }
        }
        if (l1 != nullptr) append(&last, &merged, l1);
        if (l2 != nullptr) append(&last, &merged, l2);
        return merged;
    }

    // Дополнение к списку деревьев
    void append(BinomialTreeNode** last_ref, BinomialTreeNode** merged_ref, BinomialTreeNode* node) {
        if (*last_ref == nullptr) {
            *merged_ref = node;
        } else {
            (*last_ref)->sibling = node;
        }
        *last_ref = node;
        node->sibling = nullptr;
    }

    // Консолидация (переформатирование кучи)
    void consolidate() {
        int max_degree = sizeof(int)*8;  // Максимальный возможный ранг
        std::vector<BinomialTreeNode*> aux(max_degree, nullptr);

        BinomialTreeNode* current = head;
        while (current != nullptr) {
            BinomialTreeNode* next = current->sibling;
            int d = current->degree;
            while (aux[d] != nullptr) {
                BinomialTreeNode* other = aux[d];
                if (other->key < current->key) std::swap(current, other);
                current = mergeTrees(current, other);
                aux[d] = nullptr;
                d++;
            }
            aux[d] = current;
            current = next;
        }

        head = nullptr;
        for (int i = 0; i < max_degree; i++) {
            if (aux[i] != nullptr) {
                append(&head, &head, aux[i]);
            }
        }
    }

    // Поиск минимального элемента
    int findMin() {
        if (head == nullptr) throw std::runtime_error("Heap is empty");
        int minVal = INT_MAX;
        BinomialTreeNode* current = head;
        while (current != nullptr) {
            if (current->key < minVal) minVal = current->key;
            current = current->sibling;
        }
        return minVal;
    }

    // Извлечение минимального элемента
    int extractMin() {
        if (head == nullptr) throw std::runtime_error("Heap is empty");
        int minVal = INT_MAX;
        BinomialTreeNode* minNode = nullptr;
        BinomialTreeNode* previous = nullptr;
        BinomialTreeNode* current = head;

        while (current != nullptr) {
            if (current->key < minVal) {
                minVal = current->key;
                minNode = current;
            }
            previous = current;
            current = current->sibling;
        }

        if (previous != nullptr) previous->sibling = minNode->sibling;
        else head = minNode->sibling;

        BinomialHeap childHeap;
        BinomialTreeNode* child = minNode->child;
        while (child != nullptr) {
            BinomialTreeNode* next = child->sibling;
            child->parent = nullptr;
            child->sibling = nullptr;
            childHeap.insert(child->key);
            child = next;
        }

        merge(*this, childHeap);
        return minVal;
    }

    // Освобождение памяти
    ~BinomialHeap() {
        clear();
    }

    // Очистка всех деревьев
    void clear() {
        BinomialTreeNode* current = head;
        while (current != nullptr) {
            BinomialTreeNode* next = current->sibling;
            deleteTree(current);
            current = next;
        }
        head = nullptr;
    }

    // Удаление дерева
    void deleteTree(BinomialTreeNode* node) {
        if (node == nullptr) return;
        deleteTree(node->child);
        delete node;
    }

    // Вывод кучи
    void print() {
        std::cout << "Binomial Heap:" << std::endl;
        traverseTrees(head);
    }
};

int main() {
    BinomialHeap heap;
    heap.insert(10);
    heap.insert(5);
    heap.insert(8);
    heap.insert(3);
    heap.print();
    std::cout << "Min Element: " << heap.findMin() << std::endl;
    std::cout << "Extracted Min: " << heap.extractMin() << std::endl;
    heap.print();
    return 0;
}
