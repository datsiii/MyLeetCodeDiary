# MyLeetCodeDiary
## LEETCODE TASKS FROM MY HR
### Оглавление:

linked lists:

1. https://leetcode.com/problems/merge-k-sorted-lists/
2. https://leetcode.com/problems/linked-list-cycle/
3. https://leetcode.com/problems/add-two-numbers/
4. https://leetcode.com/problems/reverse-linked-list/

binary search:

5. https://leetcode.com/problems/binary-search/
6. https://leetcode.com/problems/guess-number-higher-or-lower/
7. https://leetcode.com/problems/search-a-2d-matrix/
8. https://leetcode.com/problems/search-in-rotated-sorted-array/
9. https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
10. https://leetcode.com/problems/search-in-rotated-sorted-array-ii/

hash table:

11. https://leetcode.com/problems/single-number/ - решить за O(1) по памяти
12. https://leetcode.com/problems/two-sum/
13. https://leetcode.com/problems/4sum/
14. https://leetcode.com/problems/group-anagrams/
15. https://leetcode.com/problems/valid-anagram/
16. https://leetcode.com/problems/find-all-anagrams-in-a-string/

queue/stack:

17. https://leetcode.com/problems/valid-parentheses/

dfs/bfs:

18. https://leetcode.com/problems/number-of-islands/
19. https://leetcode.com/problems/remove-invalid-parentheses/

sort:

20. https://leetcode.com/problems/merge-intervals/

heap/hash:

21. https://leetcode.com/problems/top-k-frequent-words/
22. https://leetcode.com/problems/top-k-frequent-elements/

two pointers:

23. https://leetcode.com/problems/container-with-most-water/
24. https://leetcode.com/problems/partition-labels/

sliding window:

25. https://leetcode.com/problems/sliding-window-median/
26. https://leetcode.com/problems/sliding-window-maximum/
27. https://leetcode.com/problems/longest-repeating-character-replacement/

tree:

28. https://leetcode.com/problems/same-tree/
29. https://leetcode.com/problems/symmetric-tree/
30. https://leetcode.com/problems/balanced-binary-tree/
31. https://leetcode.com/problems/path-sum-ii/

greedy problems:

32. https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
33. https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
34. https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/
35. https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
36. https://leetcode.com/tag/prefix-sum/

### Решения

#### linked-list-cycle
2. https://leetcode.com/problems/linked-list-cycle/

    Задача на нахождения цикла в связанном списке. В решении представлен известный алгоритм на нахождение цикла "заяц и черепаха".
В чем его идея: у нас есть два указателя, и один из них двигается в два раза быстрее (заяц), и в следствие если в связанном списке действительно есть цикл, то заяц догонит черепаху в любом случае. Если же не догнал, то цикла нет.

```
class Solution {
    fun hasCycle(head: ListNode?): Boolean {
        var tortoise = head
        var hare = head
        while ((hare != null) and (hare?.next != null)) {
            tortoise = tortoise?.next
            hare = hare?.next?.next
            if (tortoise == hare) return true
        }
        return false
    }
}
```
#### add-two-numbers
3. https://leetcode.com/problems/add-two-numbers/
   
    Задача по сути на сложение двух чисел, которые представленны в виде связанного списка в обратном порядке. Обратный порядок нам только на руку?
В чем суть моего алгоритма: у нас есть два указателя, которые двигаются по двум св. спискам. Создала dummy node - res - пустую ноду, с помощью которой будем хранить результат, т.е. она будет хранить ссылку на начало указателя, ходящего по результируещему числу - новому списку. А еще есть переменная, которая будет хранить переносы (если сумма двух чисел > 9). Определяем сумму, не забываем про перенос, в ноду добавляем остаток по 10 от суммы, а в перенос целочисленно деление на 10 (но в целом можно и просто +1? Кажется макс сумма это 18). Двигаем наши указатели. В конце, после цикла, **ВАЖНО НЕ ЗАБЫТЬ** проверить на то, не осталось ли у нас переноса, и если да, то добавить еще ноду с переносом.

```
class Solution {
    fun addTwoNumbers(l1: ListNode?, l2: ListNode?): ListNode? {
        var p1 = l1
        var p2 = l2
        val res = ListNode(0)
        var cur = res
        var carry = 0
        while ((p1 != null) or (p2 != null)) {
            val sum = (p1?.`val` ?: 0) + (p2?.`val` ?: 0)
            val temp = sum + carry
            cur.next = ListNode(temp % 10)
            carry = temp / 10
            p1 = p1?.next
            p2 = p2?.next
            cur = cur.next
        }
        if (carry > 0) {
            cur.next = ListNode(carry)
        }
        return res.next
    }
}
```
#### reverse-linked-list    MEDIUM
4. https://leetcode.com/problems/reverse-linked-list/

   Задача на разворот списка, тут две версии решения: циклом и рекурсией.

**Цикл**

Идея: три переменные - предыдущее, текущее, следующее. Переопределяем для текущего числа ссылку на следующее - теперь это предыдущее число, предыдущее же число теперь текущее, текущее - это следующее, а следующее - следующее текущего. В конце возвращаем последнее предыдущее число.
```
class Solution {
    fun reverseList(head: ListNode?): ListNode? {
        var prev: ListNode? = null
        var cur = head
        var nextTmp = cur?.next
        while(cur != null){
            cur?.next = prev
            prev = cur
            cur = nextTmp
            nextTmp = cur?.next
        }
        return prev
    }
}
```
**Рекурсия**

Идея: переменная для хранения ссылки на новую голову. Условие выхода из рекурсии: если текущее null или следующее null. Новая голова - вызов рекурсии от следующей. Дошли до конца (null) и при backtracking'е меняем указатели:

Условно исходный список: 1->2->3->4->5

Итерации:

1: 1->2->3->4->5<-null<-5  // После обработки 5: 5->null, связь 4->5 разорвана

2: 1->2->3->4<-null<-4<-5  // 5->4->null, связь 3->4 разорвана

3: 1->2->3<-null<-3<-4<-5  // 5->4->3->null, связь 2->3 разорвана

4: 1->2<-null<-2<-3<-4<-5  // 5->4->3->2->null, связь 1->2 разорвана

5: 1<-null<-1<-2<-3<-4<-5  // 5->4->3->2->1->null

```
class Solution {
    fun reverseList(head: ListNode?): ListNode? {
        if (head == null || head.next == null) {
            return head
        }
        val newHead = reverseList(head.next)
        
        head.next?.next = head 
        head.next = null
        
        return newHead
    }
}
```
#### reverse-linked-list-ii
https://leetcode.com/problems/reverse-linked-list-ii/

2 часть задачи на разворот списка, теперь развернуть нужно только определенную часть, заданную индексами левой и правой границы. 

Идея: пока не дошли до левой границы, идем по списку, как только дошли - сохраняем ссылку на последний элемент до левой границы. 

После идем до правой границы, разворачиваем список как в задаче выше,  дальше связываем кусочки: определяем ссылку на следующий элемент для последнего элемента как последний элемент развернутого списка, а для ноды (чет щас сомневаюсб правильно ли назвала ее leftNode) как ссылку на первый элемент развернутого списка. 

В конце определяем, если мы разворачивали с самого начала (левая граница - 1) то возвращаем ссылку на первый элемент развернутного списка, иначе первый элемент всего модифицируемого списка

```
class Solution {
    fun reverseBetween(head: ListNode?, left: Int, right: Int): ListNode? {
        var cur = head
        var curIndx = 1
        var leftNode: ListNode? = null
        var lastel: ListNode? = null
        var prev: ListNode? = null
        if (left == right) return head
        while(curIndx < left) {
            leftNode = cur
            cur = cur?.next
            curIndx++
        }
        lastel = cur
        while(curIndx <= right) {
            val nextTmp = cur?.next
            cur?.next = prev
            prev = cur
            cur = nextTmp
            curIndx++
        }
        leftNode?.let{leftNode.next = prev}
        lastel?.next = cur
        return if (left == 1) prev else head
    }
}
```
#### merge-k-sorted-lists
1. https://leetcode.com/problems/merge-k-sorted-lists/ HARD

   Задача на слияние k отсортированных списков в один отсортированный список.
   
   Идея: 
   
```
class Solution {
    fun mergeKLists(lists: Array<ListNode?>): ListNode? {
        val res: ListNode? = ListNode(0)
        var cur = res
        val heap = PriorityQueue<ListNode>(compareBy { it.`val` })
        lists.forEach { node ->
            node?.let { heap.add(node) }
        }
        while (heap.isNotEmpty()) {
            cur?.next = heap.poll()
            cur = cur?.next
            cur?.next?.let { heap.add(cur?.next) }
        }
        return res?.next
    }
}
```
#### binary-search
5. https://leetcode.com/problems/binary-search/   EASY

   Бинарный поиск, есть два указателя, делим массив на половинки, тк массив отсортирован (а для бин поиска он должен быть отсортирован), смотрим нужно в меньшую половину или в большую (двигаем указатели)
   
```
class Solution {
    fun search(nums: IntArray, target: Int): Int {
        var l = 0
        var r = nums.size - 1
        while (l < r) {
            val i = (l+r)/2
            if (nums[i] < target) {
                l = i + 1
            } else {
                r = i
            }
        }
        return if (nums[r] == target) r else -1
    }
}
```
#### guess-number-higher-or-lower
6. https://leetcode.com/problems/guess-number-higher-or-lower/
   
   Еще один бинарный поиск, тут есть функция guess, которая говорит больше загаданное число или меньше. Важно: num = l + (r - l) / 2, используем такую формулу, чтобы избежать переполнения при сложении!

```
class Solution:GuessGame() {
    override fun guessNumber(n:Int):Int {
        var l = 0
        var r = n
        while (true) {
            val num = l + (r - l) / 2
            val pick = guess(num)
            when(pick) {
                0 -> return num
                1 -> l = num + 1
                -1 -> r = num - 1
            }
        }
    }
}
```
#### search-a-2d-matrix
7. https://leetcode.com/problems/search-a-2d-matrix/ MEDIUM

   В задаче нужно определить есть ли заданное число в матрице, причем известно, что числа в матрице упорядочены (отсортированы).

   Что ж, такая постановка условия опять наталкивает на бинарный поиск. Поскольку м знаем, что все числа упорядочены, то искать число стоит только в той строке, в котором макс число (последнее число в строке) больше, чем заданное число (Сейчас думаю, что можно так же добавить условие на то, что минимальное число в строке должно быть меньше или равно заданному числу).

   Дальше бинарным поиском определяем есть ли число в строке.
```
class Solution {
    fun searchMatrix(matrix: Array<IntArray>, target: Int): Boolean {
        val n = matrix[0].size - 1
        matrix.forEach { row ->
            var l = 0
            var r = n
            if (!(target > row[n])) {
                while (l <= r) {
                    val m = l + (r - l) / 2
                    if (row[m] < target) {
                        l = m + 1
                    } else {
                        r = m - 1
                    }
                }
                if (row[l] == target) return true
            }
        }
        return false
    }
}
```

#### search-in-rotated-sorted-array
8. https://leetcode.com/problems/search-in-rotated-sorted-array/  MEDIUM

   А тут тоже задачка на бинарный поиск, но сложнее: массив был повернут. Это нам мешает, ведь для бин поиска массив должен быть отсортирован, а тут уже не совсем. Тогда мы приходим к мысли о том, что так-то есть отдельно отсортированные части, в которых мы и можем применить бинарный поиск. Но как нам найти pivot - ту точку, которая разделяет эти отдельные части? А то же при помощи бинарного поиска: нам нужно найти такое число, которое больше числа справа от него. После нахождения pivot, нам нужно определить, в какой половине нам искать, и тут возможны три случая:
   
   1. Pivot равен нулю - это означает, что массив не был никак повернут и искать нам заданное число нужно во всем массиве
   2. Если же массив был повернут, то нужно определить больше ли заданное число минимального числа из повернутой части (причем мин число из повернутой - левой - части - явно больше любого числа неповернутой - правой - части). Если да, то ищем цель в повернутой части: от 0 до pivot - 1 (тк само число pivot - это начало правой части).
   3. В ином случае, ищем не в повернутой части: от pivot до n.

      Снова применяем бинарный поиск.

```
fun search(nums: IntArray, target: Int): Int {
        val n = nums.size - 1
        var l = 0
        var r = n
        var pivot = 0
        
        while (l < r) {
            val m = l + (r - l)/2
            if (nums[m] > nums[r]) l = m + 1
            else r = m
        }
        pivot = l
        if (nums[pivot] == target) return pivot
        
        if (pivot == 0) {
            l = 0
            r = n
        } else if (target >= nums[0]) {
            l = 0
            r = pivot - 1 
        } else {
            l = pivot
            r = n 
        }
        while (l <= r) {
            val m = l + (r - l)/2
            if(nums[m] == target) return m
            if (nums[m] < target) l = m + 1
            else r = m - 1
        }
        return -1
    }
```
#### find-minimum-in-rotated-sorted-array
9. https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/   MEDIUM

    А вот тут тоже задачка с повернутом массивом, но уже на нахождение минимума, что, в целом, делает задачу проще предыдущей, так как решение сводится к ее первой части: нахождение pivot. (Т.к. pivot - это начало неперевернутой части, точнее минимальной, а еще точнее pivot и есть минимальное число).
   
```
class Solution {
    fun findMin(nums: IntArray): Int {
        val n = nums.size - 1
        var left = 0
        var right = n
        while (left < right) {
            val mid = left + (right - left) / 2
            if (nums[mid] > nums[right]) left = mid + 1
            else right = mid
        }
        return nums[left]
    }
}
```

#### search-in-rotated-sorted-array-ii
10. https://leetcode.com/problems/search-in-rotated-sorted-array-ii/    MEDIUM

    Вторая часть задачки с поиском в повернутом массиве. Нам нужно только определить, есть ли заданное число в массиве. Но теперь проблема: числа в массиве могут повторяться. А на что это влияет? Pivot долго искать! Условие nums[left] == nums[mid] не позволяет однозначно определить, какая половина отсортирована. В худшем случае поиск pivot займет O(n), а мы хотим O(logn)

    Каков план? Все тот же бинарный поиск, но теперь сложнее логика определения следующей части поиска. Учтем условие: nums is guaranteed to be rotated at some pivot.

    Случай 1 (левая половина отсортирована): наша серединка больше, чем левая граница. В таком случае, а находится ли заданное число в диапазоне от левой границы до середины? Если да, то правую границу можно уменьшить до середины - 1. Иначе левую границу нужно увеличить до середины + 1.

    Случай 2 (правая половина отсортирована): левая граница все таки больше, чем серединка. Тогда нам стоит смотреть на правую часть: находится ли заданное число в диапазоне от середины до правой части? Если да, то левую границу нужно увеличить до середины + 1, а иначе правую границу можно уменьшить до середины - 1.

    Случай 3 (дубликаты): если левая граница равна серединке, то просто двигаем левую границу вперед.

```
class Solution {
    fun search(nums: IntArray, target: Int): Boolean {
        val n = nums.size - 1
        var left = 0
        var right = n
        while (left <= right) {
            val mid = left + (right - left) / 2
            if (nums[mid] == target) return true
            
            if (nums[left] < nums[mid]) {  
                if (target in nums[left] until nums[mid]) {
                    right = mid - 1
                } else {
                    left = mid + 1
                }
            } else if (nums[left] > nums[mid]) {  
                if (target in nums[mid]..nums[right]) {
                    left = mid + 1
                } else {
                    right = mid - 1
                }
            } else {  
                left++
            }
    }
        return false
    }
}
```
#### single-number
11. https://leetcode.com/problems/single-number/ - решить за O(1) по памяти

    Супер не очевидная и не интуитивная задачка!

    Given a non-empty array of integers nums, every element appears twice except for one. Find that single one

    Решается xor'ом. Пройдясь исключающем-или по всему массиву в итоге останется только то число, что не было повторено, тк 101 xor 101 = 000

```
class Solution {
    fun singleNumber(nums: IntArray): Int {
        var ans = 0
        nums.forEach { num ->
            ans = ans.xor(num)
        }
        return ans
    }
}
```
#### two-sum
(2sum)
12. https://leetcode.com/problems/two-sum/   EASY

Супер задачка! Дан массив и заданное число - сумма. В массиве нам нужно найти два числа, которые при сложении дают нужную нам сумму. 

Что делаем? Ну, я завела хэшмапу (map.containsKey есть такое, реализацию кажется можно улучшить). В чем идея? В хэшмапе мы храним число и его позицию, почему хэшмапа? Операция проверки нахождения в хэшмапе выполняется очень быстро. Потом проходимся по массиву чисел, и поскольку у нас всего два числа, то мы четко знаем, какое второе число нам нужно (m = target - n), его и ищем в хэшмапе.

```
class Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        val hmap = HashMap<Int,Int>()
        for (i in 0 .. nums.size) {
            val n = nums[i]
            val m = target - n
            if (m in hmap) return intArrayOf(i, hmap[m]!!)
            hmap[n] = i
        }
        return intArrayOf(0, 0)
    }
}
```
#### 3sum
(3 sum)
https://leetcode.com/problems/3sum/
 
 А вот уже вариант посложнее: нужно найти три числа nums[i] + nums[j] + nums[k] == 0. Но тут нам уже не нужны в ответе индексы, а только конкретно три числа. Хэшмапа нам тут, увы, не поможет(
 
 Что делаем? Первым делом, сортируем. Зачем? А чтобы вторым делом, двигаться двумя указателями (лево и право). Тут нам важно пропускать дубликаты, а чтобы оптимизировать код, определить условия раннего выхода.
     
Шаги:

Фиксируем первый элемент (индекс i): Для каждого элемента nums[i] ищем пару элементов (l, r) такую, что nums[i] + nums[l] + nums[r] = 0. Эквивалентно: ищем nums[l] + nums[r] = -nums[i].
     
Пропускаем дубликаты для i: если текущий совпадает с предыдущим, пропускаем его. (чтоб не было дублирующихся троек)

Два указателя (для каждого i): l = i + 1 (сразу после i), r = конец массива

Если мы нашли нужную тройку, то сразу
Пропускаем дубликаты слева (while (l < r && nums[l] == nums[l+1]) l++).
Пропускаем дубликаты справа (while (l < r && nums[r] == nums[r-1]) r--).
И сдвигаем оба указателя (l++, r--).

Если не нашли, но понимаем, что сумма меньше цели: сдвигаем l вправо (увеличиваем сумму).
А иначе сдвигаем r влево (уменьшаем сумму).

Оптимизация: если nums[i] > 0, сразу выходим — дальше все числа положительные, сумма 0 невозможна.
```
class Solution {
    fun threeSum(nums: IntArray): List<List<Int>> {
        val n = nums.size - 1
        val res = mutableListOf<List<Int>>()
        nums.sort()
        for (i in 0..n-1) {
            val target = -nums[i]
            var l = i + 1
            var r = n
            if (i > 0 && nums[i]==nums[i-1]) continue
            while (l < r) {
                if (nums[l] + nums[r] == target) {
                    res.add(listOf(nums[i], nums[l], nums[r]))
                    while (l < r && nums[l] == nums[l+1]) l++
                    while (l < r && nums[r] == nums[r-1]) r--
                    l++
                    r--
                } else if (nums[l] + nums[r] < target) {
                    l++
                } else {
                    r--
                }
            }
        }
        return res
    }
}
```
#### 4sum
(4sum)
13. https://leetcode.com/problems/4sum/

Теперь задача еще сложнее..нужны 4 числа! И числа могут быть большими, использовала .toLong() для избежания переполнения.

Идея похожа, тоже два указателя (для этого сортируем)

Два вложенных цикла для фиксации первых двух чисел: внешний цикл по i (от 0 до n-4), внутренний цикл по j (от i+1 до n-3) + оптимизация: пропуск дубликатов continue

Оптимизации перед использованием двух указателей: ранний выход если минимальная возможная сумма (nums[i] + nums[j] + nums[j+1] + nums[j+2]) больше заданной и если максимальная возможная сумма (nums[i] + nums[j] + nums[n-2] + nums[n-1]) меньше заданной

Два указателя для поиска оставшихся двух чисел: l = j+1, r = n-1 (лево и право)

Если нашли нужную сумму, пропускаем дубликаты и сдвигаем указатели: l++, r--, если сумма меньше нужной, сдвигаем l вправо (увеличиваем сумму). А иначе сдвигаем r влево (уменьшаем сумму).

```
class Solution {
    fun fourSum(nums: IntArray, target: Int): List<List<Int>> {
        val n = nums.size
        val res = mutableListOf<List<Int>>()
        nums.sort()
        for (i in 0 until n-3) {
            if (i > 0 && nums[i] == nums[i - 1]) continue
            for (j in i+1 until n-2) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue
                if (nums[i].toLong() + nums[j].toLong() + nums[j+1].toLong() + nums[j+2].toLong() > target.toLong()) break
                if (nums[i].toLong() + nums[j].toLong() + nums[n-2].toLong() + nums[n-1].toLong() < target.toLong()) continue
                var l = j + 1
                var r = n - 1
                while (l < r) {
                    val sum = nums[i].toLong() + nums[j].toLong() + nums[l].toLong() + nums[r].toLong()
                    if (sum == target.toLong()) {
                        res.add(listOf(nums[i], nums[j], nums[l], nums[r]))
                        while (l < r && nums[l] == nums[l+1]) l++
                        while (l < r && nums[r] == nums[r-1]) r--
                        l++
                        r--
                    }
                    else if (sum < target.toLong()) l++
                    else r--
                }
            }
        }
        return res
    }
}
```
#### group-anagrams
14. https://leetcode.com/problems/group-anagrams/   MEDIUM

    Группируем аннограмы: An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, using all the original letters exactly once.

    Моя идея: делаем хэшмапу, где ключ - это строка, а значение - анаграммы этой строки. Для этого для каждой строки делаем ее новый вид - превращаем в массив char и сортируем, если наш новый вид уже есть в хэшмапе - добавляем к анаграммам старый вид
    
```
class Solution {
    fun groupAnagrams(strs: Array<String>): List<List<String>> {
        val hmap = HashMap<String, MutableList<String>>()
        strs.forEach { s ->
            val newS = s.toCharArray().sortedArray().joinToString("")
            if (newS !in hmap) hmap[newS] = mutableListOf(s)
            else hmap[newS]?.add(s)
        }
        return hmap.values.toList()
    }
}
```
#### valid-anagram
15. https://leetcode.com/problems/valid-anagram/   EASY

    Проверяем две строки на анаграммность.

    Самое простое решение! Но долгое...
    
```
class Solution {
    fun isAnagram(s: String, t: String): Boolean {
        return if(s.toCharArray().sorted().joinToString() == t.toCharArray().sorted().joinToString()) true else false
    }
}
```
А вот решение быстрое. Не забудем проверочку на длину. Из условий задачи нам известно, что строки состоят только из маленьких букв английского алфавита, для каждой буковки мы можем завести счетчик в массиве. Проходимся по строчке (любой, длина же равна после проверки) и для встречи буковки в первой строки будет увеличивать счетчик (индекс высчитываем по таблице char'ов), а при встрече во второй, наоборот уменьшаем счетчик.

Если строки анаграммы - у каждой буковки счетчик должен быть 0!
```
class Solution {
    fun isAnagram(s: String, t: String): Boolean {
       if (s.length != t.length) {
            return false
        }
        val count = IntArray(26)
        for (i in s.indices) {
            count[s[i] - 'a']++
            count[t[i] - 'a']--
        }
        
        count.forEach { cnt ->
            if (cnt != 0) return false
        }
        return true
    }
}
```

#### find-all-anagrams-in-a-string
16. https://leetcode.com/problems/find-all-anagrams-in-a-string/   MEDIUM

        Given two strings s and p, return an array of all the start indices of p's anagrams in s. You may return the answer in any order.

    Что делаем? Делаем скользящее окно!
    
    Массивы частот:
    
    countP — массив из 26 нулей, хранит частоты символов в p.
    
    countW — аналогично для текущего окна в s.

    Инициализация частот: заполняем countP по строке p, заполняем countW по первому окну s[0..p.length-1].

    Если частоты равны → добавляем p1

    Уменьшаем частоту s[p1] (уходит из окна), сдвигаем p1 и p2 вправо и увеличиваем частоту s[p2] (если p2 не вышел за границы).


```
class Solution {
    fun findAnagrams(s: String, p: String): List<Int> {
        if (p.length > s.length) return emptyList()
        var p1 = 0
        var p2 = p.length - 1
        val res = mutableListOf<Int>()
        var window = s.slice(p1..p2)
        val countP = IntArray(26)
        val countW = IntArray(26)
        for(i in p.indices) {
            countP[p[i] - 'a']++ 
        }
        for(i in window.indices) {
            countW[window[i] - 'a']++ 
        }
        while (p2 < s.length) {
            if (countP contentEquals countW) res.add(p1)
            countW[s[p1] - 'a']--
            p1++
            p2++
            if (p2 < s.length) countW[s[p2] - 'a']++
        }
        return res
    }
}
```
#### valid-parentheses
17. https://leetcode.com/problems/valid-parentheses/   EASY

    А вот еще одна задачка на валидность, валдиность скобочек. Ну тут идея проста, делаем кучу, в которую кладем открывающиеся скобочки, а как только нашли закрывающуюся - сравниваем с последней скобочкой в куче, чтоб совпадали по типу.

    Не забудем про краевые случаи, когда у нас могут быть только закрывающиеся скобочки, или куча не пуста - не все скобочки были закрыты

```
class Solution {
    fun isValid(s: String): Boolean {
        if (s.length == 1) return false
        val characters = Stack<Char>()
        s.forEach { ch ->
            if ((ch == '(') or (ch == '{') or (ch == '[')) characters.push(ch)
            else if (characters.isNotEmpty()) {
                val lastCh = characters.peek()
                when {
                    (ch == ')')  &&  (lastCh != '(') -> return false
                    (ch == '}')  &&  (lastCh != '{') -> return false
                    (ch == ']')  &&  (lastCh != '[') -> return false
                }
                characters.pop()
            }
            else return false
        }
        return if (characters.isNotEmpty()) false else true
    }
}
```
#### number-of-islands
18. https://leetcode.com/problems/number-of-islands/   MEDIUM

   Задачечка на обход матрицы dfs'ом, выглядит страшно, как задачки на шахматы, но на самом деле все оки.

   В dfs'е если мы видем не 1, то выходим из рекурсии, а если 1, то помечаем увиденную единичку (можно 0 типа затопили, но я решила 3). Дальше вызываем рекурсию для всех соседних клеток, кроме диагональных.

   А вызываем мы dfs в цикле по матрице, как только находим там 1 (увеличиваем счетчик островов).

```
class Solution {
    fun numIslands(grid: Array<CharArray>): Int {
        val m = grid.size - 1
        val n = grid[0].size - 1
        var count = 0

        fun dfs(i: Int, j: Int, grid: Array<CharArray>) {
            if ((i !in 0..m) or (j !in 0..n)) return
            if (grid[i][j] != '1') return
            if (grid[i][j] == '1') grid[i][j] = '3'

            dfs(i+1, j, grid)
            dfs(i, j+1, grid)
            dfs(i-1, j, grid)
            dfs(i, j-1, grid)
        }

        for (i in 0..m) {
            for (j in 0..n) {
                if (grid[i][j] == '1') {
                    count++
                    dfs(i, j, grid)
                }
            }
        }
        return count
    }
}
```
#### remove-invalid-parentheses
19. https://leetcode.com/problems/remove-invalid-parentheses/    HARD

    И опять задачка на скобочки. Только тут нужно неправильные скобочки убрать.

    Идея: используем BFS для поиска минимального числа удалений. Каждый уровень BFS — удаление одной скобки.

    Шаги:

    Проверка уровня: для каждой строки в текущей очереди:

    Если строка валидна → добавляем в результат и помечаем found=true (флаг уровня, то есть на уровень ниже спускаться не надо для минимальности удалений).

    Если found=false → генерируем новые строки удалением каждой скобки и добавляем в очередь (если не посещена).

    Ранний выход: Если на уровне нашли валидные строки → возвращаем результат.

    Проверка валидности: Счётчик баланса (открывающие +1, закрывающие -1). Как уже проверяли в первой задачке на скобки.

    (Сложность: Экспоненциальная в худшем случае, но оптимизирована посещёнными строками)

```
class Solution {
    fun removeInvalidParentheses(s: String): List<String> {
        val res = mutableListOf<String>()
        val queue: Queue<String> = LinkedList<String>()
        queue.add(s)
        val visited = HashSet<String>()

        fun isValid(s: String): Boolean{
            var count = 0 
            s.forEach { ch ->
                when (ch) {
                    '(' -> count++
                    ')' -> count--
                }
                if (count < 0) return false
            }
            return count == 0
        }

        fun bfs(queue: Queue<String>): List<String> {
            while (queue.isNotEmpty()) {
                var found = false
                val size = queue.size
                for (i in 0 until size) {
                    val str = queue.poll()
                    if (isValid(str)) {
                        res.add(str)
                        found = true
                    }
                    if(!found) {
                        for (j in str.indices) {
                            if (str[j] in "()") {
                                val newStr = str.removeRange(j, j+1)
                                if (newStr !in visited) {
                                    visited.add(newStr)
                                    queue.add(newStr)
                                }
                            }
                        }
                    }
                }
                if (found) return res.toList()
            }
            return emptyList()
        }
        return bfs(queue)
    }
}
```
#### merge-intervals
20. https://leetcode.com/problems/merge-intervals/   MEDIUM

    Задачка на соединение интервалов, важно ее запомнить, она используеются в других задачках (не самым очевидным образом).

    Ну, у нас есть два указателя - начало и конец текущего интервала. Если начало нового интервала меньше, чем конец текущего - то эти интервалы можно объединить. А иначе мы нашли конец интервала и делаем новые текущий интервал. **ВАЖНО!!! ОЧЕНЬ ВАЖНО!!** В конце после цикла обязательно добавить к результату интервал текущий, тк мы не добавили его в цикле из-за проверки!!!
    
```
class Solution {
    fun merge(intervals: Array<IntArray>): Array<IntArray> {
        val res = mutableListOf<IntArray>()
        intervals.sortBy { it[0] }
        var curStart = intervals[0][0]
        var curEnd = intervals[0][1]
        for (i in 0..intervals.size-1) {
            if (intervals[i][0] <= curEnd) {
                curEnd = max(curEnd, intervals[i][1])
            } else {
                res.add(intArrayOf(curStart, curEnd))
                curStart = intervals[i][0]
                curEnd = intervals[i][1]
            }
        }
        res.add(intArrayOf(curStart, curEnd))
        return res.toTypedArray()
    }
}
```
#### find-the-maximum-length-of-valid-subsequence-i
https://leetcode.com/problems/find-the-maximum-length-of-valid-subsequence-i/description/?envType=daily-question&envId=2025-07-17    MEDIUM

Доп задачка, не из списка ичара, но пусть будет тут

    You are given an integer array nums.
    A subsequence sub of nums with length x is called valid if it satisfies:

(sub[0] + sub[1]) % 2 == (sub[1] + sub[2]) % 2 == ... == (sub[x - 2] + sub[x - 1]) % 2.
Return the length of the longest valid subsequence of nums.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

В чем хитрость решения этой задачки: по остатку от деления на 2 мы можем получить только 0 или 1! То есть, сумма чисел может быть либо четной, либо нечетной! Но можем ли мы определить, в каком случае сумма двух чисел будет четной или нет?

В моем алгоритме (интуитивно понятном, но не очень быстром) я решила определить четыре таких случая:

1: Сумма будет четной, если два числа четные, поэтому просто считаем все четные числа - то есть суммы всех чисел этой подпоследовательности по остатку от 2х будут давать 0

2: Сумма будет четной, если два числа нечетные, поэтому просто считаем все нечетные числа - то есть суммы всех чисел этой подпоследовательности по остатку от 2х будут давать 0

Сумма будет нечетной, если четные и нечетные числа будут чередоваться, но в таком случае важно будет понять с какого числа началась наша подпоследовательность, с четного или нет, поэтому были определены еще два случая

3: Сумма будет нечетной, если четность числа из массива будет совпадать с четностью индекса в подпоследовательности

4: Сумма будет нечетной, если четность числа из массива НЕ будет совпадать с четностью индекса в подпоследовательности

В результате берем макс от всех длин подпоследовательностей

```
class Solution {
    fun maximumLength(nums: IntArray): Int {
        val evenSub = nums.filter { it % 2 == 0 }.size
        val notEvenSub = nums.filter { it % 2 != 0 }.size
        var evenDistinctSub = 0
        var notEvenDistinctSub = 0
        for (i in 0..nums.size - 1) {
            if (evenDistinctSub % 2 == nums[i] % 2) evenDistinctSub++
            if (notEvenDistinctSub % 2 != nums[i] % 2) notEvenDistinctSub++
        }
        val result = maxOf(evenSub, notEvenSub, evenDistinctSub, notEvenDistinctSub)
        return result
    }
}
```
Более быстрое решение, чем мое:
```
class Solution {
    fun maximumLength(nums: IntArray): Int {
        var oddCount = 0
        var oddEvenCount = 0
        var evenCount = 0

        if (nums[0] % 2 == 0) {
            oddCount++
            oddEvenCount++
        } else {
            evenCount++
            oddEvenCount++
        }

        for (i in 1 until nums.size) {
            val oddness = nums[i] % 2
            when (oddness) {
                0 -> {
                    oddCount++
                    oddEvenCount += if (nums[i - 1] % 2 == 1) 1 else 0
                }
                1 -> {
                    evenCount++
                    oddEvenCount += if (nums[i - 1] % 2 == 0) 1 else 0
                }
            }
        }

        return maxOf(oddCount, oddEvenCount, evenCount)
    }
}
```
#### top-k-frequent-words
21. https://leetcode.com/problems/top-k-frequent-words/    MEDIUM

    Given an array of strings words and an integer k, return the k most frequent strings.
    Return the answer sorted by the frequency from highest to lowest. Sort the words with the same frequency by their lexicographical order.

Такс, ну тут уже задачка сложная..Конечно же, нам нужна хэшмапа и куча. Хэшмапа для быстрого нахождения и правильного хранения: ключ - слово, значение - популярность/частота встречи этого слова, куча для хранения топ k слов.

Куча..вот тут начинаются проблемы. Нам нужно определить компоратор, чтобы куча правильно сортировала внутри себя слова: compareBy { hmap[it]!! }(по возрастанию частоты) + thenByDescending { it } (при равных частотах: по убыванию слова).

Дальше проходимся по словам и заполняем хэшмапу.

Проходимся теперь по ключам хэшмапы и добавляем в кучу, если куча превысила размер k, удаляем минимальный элемент.

После добавяем все из кучи в список и реверсим результат, тк в куче порядок удобный для определния k элементов, но не в том порядке, в котором требуется ответ:
    
Извлекаем элементы из кучи (от "худших" к "лучшим")

Реверсируем список для порядка: убывание частоты + возрастание лексикографического порядка
    
```
Class Solution {
    fun topKFrequent(words: Array<String>, k: Int): List<String> {
        val hmap = HashMap<String, Int>()
        val heap = PriorityQueue( compareBy<String> { hmap[it]!! }.thenByDescending { it }) 
        words.forEach { word ->
            if (word in hmap) {
                hmap[word] = hmap[word]!! + 1
            }
            else {
                hmap[word] = 1
            }
        }

        hmap.keys.forEach { word ->
            heap.add(word)
            if (heap.size > k) heap.poll()
        }

        val result = mutableListOf<String>()
        while (heap.isNotEmpty()) result.add(heap.poll())
        return result.reversed()
    }
}
```
#### top-k-frequent-elements
22. https://leetcode.com/problems/top-k-frequent-elements/   MEDIUM

Тут тоже на топ k элементов, но задачка проще! Можем return the answer in any order.

    Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

Вообщем, делаем все тоже самое, что и выше, только в конце ничего реверсить не надо.

    
```
class Solution {
    fun topKFrequent(nums: IntArray, k: Int): IntArray {
        val hmap = HashMap<Int, Int>()
        val heap = PriorityQueue<Int>( compareBy {hmap[it]} )
        nums.forEach { num ->
            if(num in hmap.keys) hmap[num] = hmap.getOrDefault(num, 0) + 1
            else hmap[num] = 1
        }
        hmap.keys.forEach { num ->
            heap.add(num)
            if (heap.size > k) heap.poll()
        }
        return heap.toIntArray()
    }
}
```
#### container-with-most-water
23. https://leetcode.com/problems/container-with-most-water/   MEDIUM

    Неплохая задачка, суть решения: у нас два указателя. Нам тут важно учесть, что площадь равна a*b, где a - это минимальная высота (из двух высот по краям), а вот b - это расстояние между двумя этими высотами (p2-p1+1). Поэтому указатели указывают на начало и конец всего списка. И для того, чтобы понять, как нам двигать указатели, нужно определить минимальную высоту и двигать именно тот указатель, который указвает на минимальную высоту (жадный алгоритм?).
```
class Solution {
    fun maxArea(height: IntArray): Int {
        var p1 = 0
        var p2 = height.size - 1
        var res = -1
        var minHeight = 0
        while (p1 < p2) {
            if (height[p1] < height[p2]) {
                minHeight = height[p1]
                p1++
            } else {
                minHeight = height[p2]
                p2--
            }
            res = max(res, minHeight*(p2-p1+1))
        }
        return res
    }
}
```
#### partition-labels
24. https://leetcode.com/problems/partition-labels/   MEDIUM

    You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

    For example, the string "ababcc" can be partitioned into ["abab", "cc"], but partitions such as ["aba", "bcc"] or ["ab", "ab", "cc"] are invalid.

    Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.

    Return a list of integers representing the size of these parts.

Тут мы вспоминаем задачку на объединение интервалов!!

Вот это решение более универсальное. Проходимся по строке и для каждой буквы запоминаем последнюю встречу ее в строке. Дальше определяем границы интервала, начало на 0, а вот конец на последней встрече первой буквы. Опять проходимся по строке и определяем конец интервала, если мы дошли до конца интервала, то добавляем его длину (last - first + 1) в результат и начинаем новый интервал.

```
class Solution {
    fun partitionLabels(s: String): List<Int> {
        val hmap = HashMap<Char, Int>()
        val res = mutableListOf<Int>()
        for (i in s.indices) {
            hmap[s[i]] = i
        }
        var first = 0
        var last = hmap[s[0]]!!
        for (i in s.indices) {
            last = maxOf(last, hmap[s[i]]!!)
            if (i == last) {
                res.add(last - first + 1)
                first = last + 1
            }
        }
        return res
    }
}
```
Этот алгоритм работает быстрее засчет использования IntArray(26). Вообще, кажется, каждый раз, когда работаем с char, предполагается его использование, но это не универсальный вариант.
```
class Solution {
    fun partitionLabels(s: String): List<Int> {
        val result = mutableListOf<Int>()
        val positions = IntArray(26)
        for (i in s.indices) {
            val index = s[i] - 'a'
            positions[index] = i
        }
        var start = 0
        var last = 0
        for (i in s.indices) {
            last = maxOf(last, positions[s[i]-'a'])
            if (i == last) {
                result.add(last - start + 1)
                start = last + 1
            }
        }
        return result
    }
}
```
#### sliding-window-maximum
26. https://leetcode.com/problems/sliding-window-maximum/   HARD

    You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.
    Return the max sliding window.

Ну тут задачка на sliding window, еще и hard. Рассмотрим сразу хорошее решение, плохое не буду.

В хорошем решении нам нужно deque (дальше dq) - очередь, где у нас есть доступ к двум концам (Хранение индексов в порядке убывания значений).  

Мы начинаем проходится по массиву. И в перую очередь нам нужно удалить все числа в dq, которые меньше нового числа из массива, они нам не нужны (своего рода сортировка, они не могут быть максимумами). 

После добавляем в dq как последний элемент индекс нового числа (Всегда добавляем в конец).

Если мы уже не в окне (heap.first() == i - k), то удалем первый элемент (двигаем окно на один вперед). Если окно сформировано (i >= k - 1), добавляем к результату макс число в окне.

плохое, но простое решение:
```
class Solution {
    fun maxSlidingWindow(nums: IntArray, k: Int): IntArray {
        val result = mutableListOf<Int>()
        val heap = PriorityQueue<Int>(compareByDescending{it})
        for (i in 0..k-1) {
            heap.add(nums[i])
        }
        result.add(heap.peek())
        var p1 = 0
        var p2 = k - 1
        while (p2 < nums.size - 1) {
            heap.remove(nums[p1])
            p1++
            p2++
            heap.add(nums[p2])
            result.add(heap.peek())
        }
        return result.toIntArray()
    }
}
```
хорошее решение с deque
```
class Solution {
    fun maxSlidingWindow(nums: IntArray, k: Int): IntArray {
        val result = mutableListOf<Int>()
        val heap = ArrayDeque<Int>() //на самом деле это deque, не путать с кучей!! я неправильно назвала
        for (i in nums.indices) {
            while (heap.isNotEmpty() && nums[i] > nums[heap.last()]) {
                heap.removeLast()
            }
            heap.addLast(i)
            if (heap.first() == i - k) {
                heap.removeFirst()
            }
            if (i >= k - 1) {
                result.add(nums[heap.first()])
            }
        }
        return result.toIntArray()
    }
}
```
#### longest-repeating-character-replacement
27. https://leetcode.com/problems/longest-repeating-character-replacement/    MEDIUM
    You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

    Return the length of the longest substring containing the same letter you can get after performing the above operations.

Задача на буквы, значит что? Используем IntArray(26). Опять sliding window. В чем ключевая идея решения? Интутитивно понятно, что нам нужна строка, в которой только k кол-во букв отличается от самой популярной буквы!

Проходимся по массиву, для встреченной буквы увеличиваем счетчик и определяем максимальный счетчик (счетчик самой популярной буквы), вместе с этим увеличиваем window. 

Если уже больше чем k букв отличается от самой популярной (((endIndex - startIndex) - maxCount) > k), то двигаем окно на 1 букву вперед. При этом, просто уменьшаем счетчик буквы у начальной границы окна на один.

Правка: endIndex можно заменить i
```
class Solution {
    fun characterReplacement(s: String, k: Int): Int {
        val arr = IntArray(26)
        var maxCount = 0
        var startIndex = 0
        var endIndex = 0
        var result = 0
        
        for (i in s.indices) {
            arr[s[i]-'A']++
            maxCount = maxOf(maxCount, arr[s[i] - 'A'])
            endIndex++

            if (((endIndex - startIndex) - maxCount) > k) {
                arr[s[startIndex] - 'A']--
                startIndex++
            }

            result = max(result, endIndex - startIndex)
        }
        return result
    }
}
```
#### same-tree
28. https://leetcode.com/problems/same-tree/

Ну просто проходимся рекурсией по дереву и проверяем, что хождение по левым нодам равно хождению по правым нодам (return isSameTree(p.left, q.left) && isSameTree(p.right, q.right))

мой старый вариант
```
class Solution {
    fun isSameTree(p: TreeNode?, q: TreeNode?): Boolean {
        var result = true
        fun dfs(p: TreeNode?, q: TreeNode?) {
            if ((p == null) and (q == null)) return
            dfs(p?.left, q?.left)
            if (p?.`val` != q?.`val`) result = false
            dfs(p?.right, q?.right)
        }
        dfs(p, q)
        return result
    }
}
```
новый вариант лучше
```
class Solution {
    fun isSameTree(p: TreeNode?, q: TreeNode?): Boolean {
        if (p == null && q == null) return true
        if (p == null || q == null) return false
        if (p.`val` != q.`val`) return false
        
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right)
    }
}
```
#### symmetric-tree
29. https://leetcode.com/problems/symmetric-tree/    EASY

Похоже на предыдущую задачку, только тут мы сравниваем лево с право у разных нод (return isSymmetricNodes(p.left, q.right) && isSymmetricNodes(p.right, q.left))
```
class Solution {
    fun isSymmetric(root: TreeNode?): Boolean {

        fun isSymmetricNodes(p: TreeNode?, q: TreeNode?): Boolean {
            if (p == null && q == null) return true
            if (p == null || q == null) return false
            if(p?.`val` != q.`val`) return false

            return isSymmetricNodes(p.left, q.right) && isSymmetricNodes(p.right, q.left)
        }

        return isSymmetricNodes(root?.left, root?.right)
    }
}
```
#### balanced-binary-tree
30. https://leetcode.com/problems/balanced-binary-tree/    EASY
    A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

Тут сложно. Условие выхода из рекурсии - мы дошли до листка - нода равна null - возвращам 0. Определяем лево и право (dfs(node.left) и dfs(node.right)). Ищем разницу между ними, если больше 1, то все, дерево не сбалансированно. А так, возвраащем максимум от лева и права + 1 (определяем высоту так).
```
class Solution {
    fun isBalanced(root: TreeNode?): Boolean {
        
        fun dfs(node: TreeNode?): Int {
            if (node == null) return 0

            val leftHeight = dfs(node.left)
            if (leftHeight == -1) return -1

            val rightHeight = dfs(node.right)
            if (rightHeight == -1) return -1

            if(abs(rightHeight - leftHeight) > 1) return -1

            return max(leftHeight, rightHeight) + 1
        }
        
        return dfs(root) != -1
    }
}
```
более понятный код
```
class Solution {
    private var isBalanced = true

    fun isBalanced(root: TreeNode?): Boolean {
        dfs(root)
        return isBalanced
    }

    private fun dfs(node: TreeNode?): Int {
        if (node == null) return 0
        val left = dfs(node.left)
        val right = dfs(node.right)

        if (abs(left - right) > 1) {
            isBalanced = false
        }

        return maxOf(left, right) + 1
    }
}
```
#### path sum I
https://leetcode.com/problems/path-sum/    MEDIUM

Тут задачка, в которой нужно определить есть ли root-to-leaf!!! путь, значения нод котрого в сумме дают targetSum. Пока обходим дерево newSum = currentSum + node.`val, когда дошли до листка (node.left == null && node.right == null) сравниваем суммы. Рекурсивно вызываем return dfs(node.left, newSum) || dfs(node.right, newSum): для не листьев → проверяем левое и правое поддеревья. Логика ИЛИ: Если хотя бы один путь в потомках валиден → возвращаем true.

```
class Solution {
    fun hasPathSum(root: TreeNode?, targetSum: Int): Boolean {
        fun dfs(node: TreeNode?, currentSum: Int) : Boolean {
            if (node == null) return false
            val newSum = currentSum + node.`val`
            if (node.left == null && node.right == null) {
                return newSum == targetSum
            }
            return dfs(node.left, newSum) || dfs(node.right, newSum)
        }
        return dfs(root, 0)
    }
}
```

#### path sum II
31. https://leetcode.com/problems/path-sum-ii/    MEDIUM

Вторая часть той же задачки. Теперь нужно вернуть all root-to-leaf paths.

Добавляем текущий узел в путь: newSum = currentSum + node.val`` ии temp.add(node.val)

Проверяем на листок и сумму: если узел — лист и newSum == targetSum → добавляем копию temp в result.

Рекурсивный вызов: dfs(node.left, newSum) и dfs(node.right, newSum)

Backtracking: Удаляем последний элемент из temp (temp.removeAt(temp.size-1)).

**Важное замечание!!** Использование ArrayList(temp) критически важно, потому что temp изменяется в процессе backtracking. Без копии все пути в результате будут одинаковыми (последним состоянием temp)!!
```
class Solution {
    fun pathSum(root: TreeNode?, targetSum: Int): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        val temp = mutableListOf<Int>()
        fun dfs(node: TreeNode?, currentSum: Int) {
            if (node == null) return

            val newSum = currentSum + node.`val`
            temp.add(node.`val`)

            if (node.left == null && node.right == null && newSum == targetSum) {
                result.add(ArrayList(temp)) //копируем!
            }

            dfs(node.left, newSum)
            dfs(node.right, newSum)
            temp.removeAt(temp.size - 1)
        }

        dfs(root, 0)
        return result
    }
}
```
#### best-time-to-buy-and-sell-stock
32. https://leetcode.com/problems/best-time-to-buy-and-sell-stock/    EASY

Хорошая задачка) Нужно вернуть макс профит от продажи акций. Решение: находим день, в котром акция стоит минимально. А результатом будет наибольшая разница между текущей ценой и ценой в минимальный день.
```
class Solution {
    fun maxProfit(prices: IntArray): Int {
        var result = 0
        var minDay = prices[0]

        for (i in prices.indices) {
            minDay = minOf(minDay, prices[i])
            result = maxOf(result, prices[i] - minDay)
        }

        return result
    }
}
```

#### best-time-to-buy-and-sell-stock-ii    MEDIUM
33. https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/

Еще одна хорошая задачка, точнее ее вторая часть. Теперь мы можем несколько раз покупать и продавать акции. Хитрость решения: мы просто продаем и покупаем акции каждый раз, когда в плюсе (prices[i+1] > prices[i]), даже если маленьком, в сумме это даст нам макс профит.
```
class Solution {
    fun maxProfit(prices: IntArray): Int {
        var profit = 0

        for (i in 0 until prices.size - 1) {
            if (prices[i+1] > prices[i]) {
                profit += prices[i+1] - prices[i]
            }
        }
    
        return profit
    }
}
```
#### best-time-to-buy-and-sell-stock-with-transaction-fee
34. https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/    MEDIUM

Версия хорошей задачки с налогом. Важно: налог только при продаже. У нас есть два состояния: либо продаем (и у нас cash), либо держим (hold, покупаем)

hold[0] = -prices[0] инициализируем так, поскольку мы купили первую акцию в долг.

Из cash мы можем либо ничего не делать (cash[i-1]), либо еще раз продать (hold[i-1] + prices[i] - fee).

Из hold мы можем либо ничего не делать (hold[i-1], либо купить (cash[i-1] - prices[i]).

Нужен максимум.
```
class Solution {
    fun maxProfit(prices: IntArray, fee: Int): Int {
        val n = prices.size
        var profit = 0

        val cash = IntArray(n)
        val hold = IntArray(n)
         
        cash[0] = 0
        hold[0] = -prices[0]

        for (i in 1..n - 1) {
            cash[i] = maxOf(cash[i-1], hold[i-1] + prices[i] - fee)
            hold[i] = maxOf(hold[i-1], cash[i-1] - prices[i])
            profit = maxOf(cash[i], hold[i])
        }

        return profit
    }
}
```
более быстрое решение
Нам не обязательно в этой задаче нужен массив!! можем оперировать переменными!
```
class Solution {
    fun maxProfit(prices: IntArray, fee: Int): Int {
        val n = prices.size
         
        var sold = 0
        var buy = -prices[0]

        for (i in 1..n - 1) {
            val prevSold = sold
            sold = maxOf(prevSold, buy + prices[i] - fee)
            buy = maxOf(buy, prevSold - prices[i])
        }

        return sold
    }
}
```
#### best-time-to-buy-and-sell-stock-with-cooldown
35. https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/    MEDIUM

Версия хорошей задачки с cooldown'ом. Уже ученые, что массив нам не нужен, оперируем переменными:

hold - из этого состояния мы можем что-нибудь купить или ничего не делать (maxOf(prevHold, cooldown - prices[i]))

sold = 0 - в этом состоянии мы продаем, из него мы обязательно должны перейти в cooldown (prevHold + prices[i])

cooldown - из этого состояния мы либо можем продолжать отдыхать, либо что-нибудь купить (maxOf(cooldown, prevSold)) (Могли отдыхать и вчера → prevCooldown или завершили вчерашнюю продажу → prevSold)

Ответ: max(sold, cooldown) (в конце нельзя иметь акцию, иначе убыток).

```
class Solution {
    fun maxProfit(prices: IntArray): Int {
        val n = prices.size

        var cooldown = 0//may buy or do nothing
        var hold = -prices[0]//may buy or hold
        var sold = 0 //sell, cooldown after that

        for (i in 1..n - 1) {
            val prevHold = hold
            val prevSold = sold

            hold = maxOf(prevHold, cooldown - prices[i])
            sold = prevHold + prices[i]
            cooldown = maxOf(cooldown, prevSold)
        }

        return maxOf(sold, cooldown)
    }
}
```
#### minimum-size-subarray-sum
https://leetcode.com/problems/minimum-size-subarray-sum/?envType=problem-list-v2&envId=prefix-sum    MEDIUM
    
    Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. 
    If there is no such subarray, return 0 instead.

Ну задачка тоже на окно. Если текущая сумма больше target, уменьшаем окно. (currentSum -= nums[startIndex], startIndex++)

subLength = n+3 - чтобы определить нашли ли мы нужный подмассив или нет
```
class Solution {
    fun minSubArrayLen(target: Int, nums: IntArray): Int {
        val n = nums.size
        var subLength = n+3

        var startIndex = 0
        var currentSum = 0
        for (i in 0..n-1) {
            currentSum += nums[i]
            while (currentSum >= target) {
                subLength = min(subLength, i - startIndex + 1)
                currentSum -= nums[startIndex]
                startIndex++
            }
        }
        
        return if(subLength != n+3) subLength else 0
    }
}
```
#### find-pivot-index
https://leetcode.com/problems/find-pivot-index/?envType=problem-list-v2&envId=prefix-sum    EASY
    
    The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right.

Ну не так уж и easy.. Используем префиксные суммы. Первый раз проходимся по массиву и заполняем их. Второй раз проходимся по префиксным суммам и сравниваем левую и правую сумму. pivot = i - 1!!
```
class Solution {
    fun pivotIndex(nums: IntArray): Int {
        val n = nums.size
        val prefixSum = IntArray(n+1)

        prefixSum[0] = nums[0]
        prefixSum[n] = 0
        for (i in 1..n-1) {
            prefixSum[i] = prefixSum[i-1] + nums[i]
        }
        var pivot = -1
        var leftSum = 0
        var rightSum = prefixSum[n-1] - prefixSum[0]
        for (i in 1..n) {
            if (leftSum == rightSum) {
                pivot = i - 1
                break
            }
            leftSum = prefixSum[i-1]
            rightSum = prefixSum[n-1] - prefixSum[i]
        }

        return pivot
    }
}
```
#### product-of-array-except-self
https://leetcode.com/problems/product-of-array-except-self/?envType=problem-list-v2&envId=prefix-sum    MEDIUM

    Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

Вот это трешик. Используем префиксы произведения (важно: answer[i] = answer[i-1]*nums[i-1]). Дальше идем с конца и меняем тот же префиксный массив (answer[i] = answer[i] * right)! answer[i] - левая часть, определяем правую часть nums[i] * right

```
class Solution {
    fun productExceptSelf(nums: IntArray): IntArray {
        val n = nums.size
        var answer = IntArray(n)
        answer[0] = 1
        for (i in 1..n-1) {
            answer[i] = answer[i-1]*nums[i-1]
        }
        var right = 1
        for (i in n-1 downTo 0) {
            answer[i] = answer[i] * right
            right = nums[i] * right
        }

        return answer
    }
}
```

#### longest-common-subsequence
https://leetcode.com/problems/longest-common-subsequence/    MEDIUM

    Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.
    A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

Тут динамическое программирование. Проходимся по двум строками и сравниваем. Если символы равны, то увеличиваем счетчик подпоследовательности (answer[i][j] = answer[i-1][j-1] + 1). В другом случае идем на клетку с макс счетчиком (answer[i][j] = maxOf(answer[i-1][j], answer[i][j-1])). Последний элемент матрицы и будет макс ответом.

**Важно:** answer = Array(n+1) { IntArray(m+1) }, не забываем + 1!! и range 1..n, 1..m
```
class Solution {
    fun longestCommonSubsequence(text1: String, text2: String): Int {
        val n = text1.length
        val m = text2.length
        val answer = Array(n+1) { IntArray(m+1) }
        for (i in 1..n) {
            for (j in 1..m) {
                if(text1[i-1] == text2[j-1]) {
                    answer[i][j] = answer[i-1][j-1] + 1
                } else {
                    answer[i][j] = maxOf(answer[i-1][j], answer[i][j-1])
                }
            }
        }

        return answer[n][m]
    }
}
```
#### lowest-common-ancestor-of-a-binary-tree
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/    Medium

    Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
    According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

Рекурсивная функция dfs обходит дерево, начиная с корня.

Если текущий узел null, возвращаем null.

Если текущий узел равен p или q, возвращаем его (базовый случай).

Рекурсивно вызываем dfs для левого поддерева.

Рекурсивно вызываем dfs для правого поддерева.

Если оба поддерева вернули не-null, значит, p и q найдены в разных ветвях → текущий узел LCA.

Если только левое поддерево вернуло узел, возвращаем его (LCA в левой ветви).

Если только правое поддерево вернуло узел, возвращаем его.

Если оба поддерева вернули null, возвращаем null.

Результат — узел, возвращённый вызовом dfs(root).

Ключевая идея: LCA — первый узел, который "видит" оба искомых узла в своих поддеревьях.

```
class Solution {
    fun lowestCommonAncestor(root: TreeNode?, p: TreeNode?, q: TreeNode?): TreeNode? {
        fun dfs (node: TreeNode?): TreeNode? {
            if (node == null) return null
            if (node == p || node == q) return node
            val left = dfs(node.left)
            val right = dfs(node.right)
            when {
                left != null && right != null -> return node
                left != null && right == null -> return left
                else -> return right
            }
        }
        return dfs(root)
    }
}
```

## КАКИЕ-ТО ЗАДАЧКИ С ТРЕНЕРОВОК
```
//является ли первая строка подпоследовательностью второй строки
    fun fuzzySearch(input: String, text: String) : Boolean {
        var j = 0
        for (i in text.indices) {
            if (j < input.length && input[j] == text[i]) {
                j++
            }
        }
        return j == input.length
    }
    //максимальный подмассив из единиц с не более чем k изменений
    fun findMaxLengthOfOnes(nums: IntArray, k: Int): Int {
        var left = 0
        var zeroCount = 0
        var maxLength = 0

        for (i in nums.indices) {
            if (nums[i] == 0) zeroCount++
            while (zeroCount > k) {
                if (nums[left] == 0) zeroCount--
                left++
            }
            maxLength = maxOf(maxLength, i - left)
        }

        return maxLength
    }
    //ранее окно для встречи двух челиков
    fun findMeetingSlot(
        slotsA: Array<IntArray>,
        slotsB: Array<IntArray>,
        duration: Int
    ): IntArray {
        var p1 = 0
        var p2 = 0
        var window = intArrayOf(0, 0)

        val sortedA = slotsA.sortedBy { it[0] }
        val sortedB = slotsB.sortedBy { it[0] }

        while (p1 < slotsA.size && p2 < slotsB.size) {
            val a = sortedA[p1]
            val b = sortedB[p2]

            val start = maxOf(a[0], b[0])
            val end = minOf(a[1], b[1])
            if (end - start >= duration) {
                window = intArrayOf(start, start + duration)
                break
            }

            if (a[1] < b[1]) {
                p1++
            } else {
                p2++
            }
        }

        return window
    }
    //найти всплески пользовательской активности

    class RobotStatistics {
        private val eventQueue: Deque<Pair<Long, Int>> = LinkedList()// Очередь для хранения событий (время, user_id)
        private val userEventCounts = mutableMapOf<Int, Int>()// Счетчик событий для каждого пользователя
        private var robotCount = 0// Счетчик пользователей с ≥1000 событий

        fun onEvent(now: Long, userId: Int) {
            removeOldEvents(now)

            eventQueue.add(now to userId)
            val newCount = userEventCounts.getOrDefault(userId, 0) + 1
            userEventCounts[userId] = newCount

            if (newCount == 1000) {
                robotCount++
            }
        }
        fun getRobotCount(now: Long): Int {
            removeOldEvents(now)
            return robotCount
        }

        private fun removeOldEvents(now: Long) {
            val threshold = now - 300
            while (eventQueue.isNotEmpty() && eventQueue.peek().first < threshold) {
                val (time, userId) = eventQueue.poll()
                val oldCount = userEventCounts[userId] ?: 0

                if (oldCount > 0) {
                    val newCount = oldCount - 1
                    userEventCounts[userId] = newCount

                    if (oldCount == 1000) {
                        robotCount--
                    }

                    if (newCount == 0) {
                        userEventCounts.remove(userId)
                    }
                }
            }
        }
    }


    //найти два одинаковых поддерева (ну типа знаем)

    //вертикальная ось симметрии
    class Point(val x: Int, val y: Int)

    fun findVerticalAxisOfSymmetry(points: List<Point>): List<Point> {
        if (points.isEmpty()) return emptyList()

        var minX = points[0].x
        var maxX = points[0].x
        for (point in points) {
            if (point.x < minX) minX = point.x
            if (point.x > maxX) maxX = point.x
        }

        val total = minX + maxX
        if (total % 2 != 0) return emptyList()
        val axisX = total / 2

        val freqMap = mutableMapOf<Pair<Int, Int>, Int>()
        for (point in points) {
            val key = point.x to point.y
            freqMap[key] = freqMap.getOrDefault(key, 0) + 1
        }

        for ((key, count) in freqMap) {
            val (x, y) = key

            if (x == axisX) continue

            val symX = 2 * axisX - x
            val symCount = freqMap.getOrDefault(symX to y, 0)

            if (count != symCount) return emptyList()
        }

        return listOf(Point(axisX, 0), Point(axisX, 1))
    }



    //1) дан массив чисел (инты), нужно вывести минимальное произведение двух из них
    //числа же могут быть отрицательными
    fun minProduct(nums: List<Int>): Int {
        var min1 = Int.MAX_VALUE
        var min2 = Int.MAX_VALUE
        var max1 = Int.MIN_VALUE
        var max2 = Int.MIN_VALUE

        for (num in nums) {
            if (num < min1) {
                min2 = min1
                min1 = num
            } else if (num < min2) {
                min2 = num
            }
            if (num > max1) {
                max2 = max1
                max1 = num
            } else if (num > max2) {
                max2 = num
            }
        }

        return minOf(
            min1.toLong() * min2.toLong(), 
            max1.toLong() * max2.toLong(), 
            min1.toLong() * max1.toLong())
            .toInt()
    }
    

    /*
     * 2) дано 2 массива, нужно вывести массив такого же размера, где i-ый элемент - эти количество повторений до него. Дубликаты не считаются
     *
     * Пример
     * Ввод:
     * 1 2 3 4 5 6 7
     * 3 4 2 1 1 5 7
     * Вывод:
     * 0 0 2 4 4 5 6
     *
     * Объяснение:
     *
     * первый «0», тк 1 не равно 3
     *
     * второй «0», тк до этого во втором массиве не было «2» и в первом не было «4»
     *
     * «2», потому что во 2 массиве была «3» и в первом массиве была «2»
     *
     * «4», потому что к предыдущей двойки в массиве ответов прибавили ещё 2 (тк «4» есть во втором массиве и «1» есть в первом)
     *
     * «4» снова, потому что «1» уже посчитали, а «5» не было
     *
     * «5» - встретили 5 во 2 массиве, то есть прибавляем 1 к 4
     *
     * «6» - прибавляем ещё 1, тк у нас родниковые вхождения
     */

    fun countRepeats(nums1: List<Int>, nums2: List<Int>): List<Int> {
        val set1 = mutableSetOf<Int>()
        val set2 = mutableSetOf<Int>()
        var commonCount = 0
        val result = mutableListOf<Int>()
        for (i in nums1.indices) {
            val num1 = nums1[i]
            val num2 = nums2[i]

            if (num1 !in set1) {
                set1.add(num1)
                if (num1 in set2) {
                    commonCount++
                }
            }

            if (num2 !in set2) {
                set2.add(num2)
                if (num2 in set1) {
                    commonCount++
                }
            }

            result.add(commonCount)
        }

        return result
    }

    //подотрезок с суммой x
    //найти непустой подотрезок - непрерывную подпоследовательность с заданной суммой х либо сказать что это невозможно (с моего первого собеса)
    //числа могут быть отрицательными
    fun findSubsequence(nums: List<Int>, x: Int): List<Int> {
        var sum = 0
        val prefixSums = HashMap<Int, Int>()

        for (i in nums.indices) {
            sum += nums[i]
            if (prefixSums.contains(sum - x)) {
                val start = prefixSums[sum - x]!! + 1
                return listOf(start, i + 1)
            }
            prefixSums[i] = sum
        }
        return emptyList()
    }

```
#### 22. Generate Parentheses
https://leetcode.com/problems/generate-parentheses/   MEDIUM

Идея: используем рекурсию для перебора всех валидных комбинаций скобок.

Параметры функции generate:

open: количество оставшихся открывающих скобок (

close: количество оставшихся закрывающих скобок )

str: текущая строка

Логика:

База рекурсии: Если строка достигла длины 2n → добавляем в результат.

Если есть открывающие скобки (open > 0): добавляем ( и рекурсивно вызываем generate(open-1, close, str + '(').

Если закрывающих скобок больше, чем открывающих (close > open): добавляем ) и рекурсивно вызываем generate(open, close-1, str + ')').

Почему это работает: условие close > open гарантирует, что закрывающая скобка не будет поставлена раньше открывающей (например, ()) — невалидно).

Пример:
Возможные пути:
- Начать с "(" → 
  - Добавить "(" → "((" → 
    - Добавить ")" → "(()" → 
      - Добавить ")" → "(())" ✅
  - Добавить ")" → "()" → 
    - Добавить "(" → "()(" → 
      - Добавить ")" → "()()" ✅
```
class Solution {
    fun generateParenthesis(n: Int): List<String> {
        val res = mutableListOf<String>()
        fun generate(open: Int, close: Int, str: String) {
            if (str.length == n*2) {
                res.add(str)
                return
            }
            if (open > 0) {
                generate(open - 1, close, str + '(')
            }
            if (close > open) {
                generate(open, close - 1, str + ')')
            }
        }
        var str = ""
        generate(n, n, str)
        return res
    }
}
```
#### 17. Letter Combinations of a Phone Number
https://leetcode.com/problems/letter-combinations-of-a-phone-number/    MEDIUM

Ну можно решить быстрее, но я решила так.
Решение использует DFS-рекурсию для генерации всех комбинаций букв, соответствующих цифрам в digits, перебирая для каждой цифры её буквы и накапливая текущую строку, с неявным backtracking за счёт передачи строки по значению.

Backtracking в этом решении проявляется в том, что после возврата из рекурсивного вызова (который обрабатывает все комбинации для текущего префикса str + letter), мы возвращаемся к оригинальному значению str (которое не изменялось) и переходим к следующей букве. Это классический приём в DFS для комбинаторных задач.

```
class Solution {
    fun letterCombinations(digits: String): List<String> {
        val n = digits.length
        val phoneNum = HashMap<Int, String>()
        fillPhoneNumbers(phoneNum)
        val result = mutableListOf<String>()

        fun generate(str: String, index: Int) {
            if (index == n) {
                result.add(str)
                return
            }
            val digit = digits[index] - '0'
            for (letter in phoneNum[digit] ?: "") { 
                generate(str + letter, index + 1) 
            }
        }

        if (digits != "")  generate("", 0)

        return result
    }

    fun fillPhoneNumbers(phoneNum: HashMap<Int, String>){
        var str = "abcdefghijklmnopqrstuvwxyz"
        for (i in 2..9) {
            if (i == 7 || i == 9) {
                phoneNum[i] = str.slice(0..3)
                str = str.removeRange(0..3)
            } else {
                phoneNum[i] = str.slice(0..2)
                str = str.removeRange(0..2)
            }
        }
    }
}
```

#### 12. Integer to Roman 
https://leetcode.com/problems/integer-to-roman/    MEDIUM
мое решение
```
class Solution {
    fun intToRoman(num: Int): String {
        val values = arrayOf(1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1)
        val symbols = arrayOf("M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I")

        var romanInteger = ""

        var i = 0
        var n = num
        while(n > 0 && i < values.size) {
            while (n >= values[i]) {
                romanInteger += symbols[i]
                n -= values[i]
            }
            i++
        }

        return romanInteger
    }
}
```


## ЗАМЕТКИ И ЛАЙФХАКИ

### Структуры

HashMap, PriorityQueue, ArrayDeque, Queue as LinkedList

### Оценка сложности
?

### Подстрока и подпоследовательность

Подпоследовательность (subsequence) это НЕ обязательно идущие подряд символы! А вот подстрока (substring) - это НЕПРЕРЫВНАЯ подпоследовательность

### dfs, bfs, bst

dummy node, backtracking

### in order, pre order, post order

end etc

### Префиксные (суфиксные) суммы

?

## КОГДА-ТО РЕШЕННЫЕ МНОЮ ЗАДАЧКИ

Тут возможны повторения с задачками выше

```
class Solution {
    companion object {
        //88. Merge Sorted Array----------------------------------------------------------------------------------------
        fun merge(nums1: IntArray, m: Int, nums2: IntArray, n: Int): Unit {
            var i = m - 1 //индекс nums1
            var j = n - 1 //индекс nums2
            var l = m + n - 1 //указатель
            while (j >= 0) {
                if (i < 0 || nums1[i] == nums2[j] || nums1[i] < nums2[j]) {
                    nums1[l] = nums2[j]
                    j--

                } else {
                    nums1[l] = nums1[i]
                    i--
                }
                l--
            }
        }
```

```

        //27. Remove Element--------------------------------------------------------------------------------------------
        fun removeDuplicates(nums: IntArray): Int {
            var (k, j) = arrayOf(1, 0)
            for (i in 0..<nums.size - 1) {
                if (nums[i] != nums[i + 1]) {
                    j++
                    nums[j] = nums[i + 1]
                    k++

                }
            }
            return k
        }

```

```
        //169. Majority Element-----------------------------------------------------------------------------------------
        fun majorityElement(nums: IntArray): Int {
            var candidate: Int? = null
            var cnt = 0
            for (n in nums) {
                if (cnt == 0) {
                    candidate = n
                }
                cnt = if (candidate == n) cnt + 1 else cnt - 1
            }
            return candidate!!
        }
```

```
        //189. Rotate Array---------------------------------------------------------------------------------------------
        //мое решение
        fun rotate(nums: IntArray, k: Int): Unit {
            val n = nums.size
            val k = k % n
            var f = 0
            var l = k - 1
            nums.reverse()
            for (i in 1..k / 2) {
                val temp = nums[l]
                nums[l] = nums[f]
                nums[f] = temp
                f++
                l--
            }
            f = k
            l = n - 1
            for (i in 1..(n - k) / 2) {
                val temp = nums[l]
                nums[l] = nums[f]
                nums[f] = temp
                f++
                l--
            }
        }

        //более красивое решение
        fun IntArray.rotate(start: Int = 0, end: Int = this.size - 1) {
            var left = start
            var right = end
            while (left < right) {
                this[left] = this[right].also { this[right] = this[left] }
                left++
                right--
            }
        }

        fun rotate2(nums: IntArray, k: Int) {
            val n = nums.size
            val steps = k % n
            nums.rotate()
            nums.rotate(0, steps - 1)
            nums.rotate(steps, n - 1)
        }
```
```
        //121. Best Time to Buy and Sell Stock--------------------------------------------------------------------------
        //мое решение
        fun maxProfit(prices: IntArray): Int {
            var minPrice = Int.MAX_VALUE
            var maxProfit = Int.MIN_VALUE
            prices.forEach { price ->
                minPrice = minOf(minPrice, price)
                maxProfit = maxOf(maxProfit, price - minPrice)
            }
            return maxProfit
        }

        //красивое решение
        fun maxProfit2(prices: IntArray): Int {
            prices.foldIndexed<Int>(prices[0]) { i, min, n ->
                prices[i] = n - min
                if (n < min) n else min
            }
            return prices.max() ?: 0
        }
```
```
        //122. Best Time to Buy and Sell Stock II-----------------------------------------------------------------------
        fun maxProfit3(prices: IntArray): Int {
            var sum = 0
            for (i in 0..<prices.size - 1) {
                val dif = prices[i + 1] - prices[i]
                sum = if (dif > 0) sum + dif else continue
            }
            return sum
        }
```
```
        //443. String Compression---------------------------------------------------------------------------------------
        fun compress(chars: CharArray): Int {
            var r = 0
            var w = 0
            while (r < chars.size) {
                var cnt = 0
                val char = chars[r]
                while (r < chars.size && chars[r] == char) {
                    r++
                    cnt++
                }
                chars[w++] = char
                if (cnt > 1) {
                    for (dig in cnt.toString()) {
                        chars[w++] = dig
                    }
                }

            }

            return w
        }

```
```
        //14. Longest Common Prefix-------------------------------------------------------------------------------------
        fun longestCommonPrefix(strs: Array<String>): String {
            var result = ""
            strs.sort()
            val first = strs[0]
            val last = strs[strs.size - 1]
            for (i in first.indices) {
                result = if (first[i] == last[i]) result + first[i] else break
            }
            return result
        }

```
```
        //1493. Longest Subarray of 1's After Deleting One Element-------------------------------------------------------
        fun longestSubarray(nums: IntArray): Int {
            var result = 0
            var left = 0
            var right = 0
            var zero = -1

            while (right < nums.size) {
                if (nums[right] != 1) {
                    left = zero + 1
                    zero = right
                }
                result = max(result, right - left)
                right++
            }

            return result
        }
```
```
        //2996. Smallest Missing Integer Greater Than Sequential Prefix Sum---------------------------------------------
        fun missingInteger(nums: IntArray): Int {
            var sum = 0
            var last = 0
            for (i in 1..<nums.size) {
                if (nums[i] != nums[i - 1] + 1) break
                last = i
            }
            sum = nums.slice(0..last).sum()
            while (sum in nums.toSet()) {
                sum++
            }
            return sum
        }
```
```
        //[1,3,5,6,2...]-----------------------------------------------------------------------------------------------
        fun findMinimum(nums: IntArray, k: Int): String? {
            val heap = PriorityQueue<Int>(compareByDescending { it })
            if (nums.size <= k) {
                return null
            }
            nums.forEach { n ->
                if (heap.size < k) {
                    heap.add(n)
                } else if (n < heap.peek()) {
                    heap.poll()
                    heap.add(n)
                }
            }
            return heap.joinToString()
        }
```
```
        //206. Reverse Linked List-------------------------------------------------------------------------------------
        /**
         * Example:
         * var li = ListNode(5)
         * var v = li.`val`
         * Definition for singly-linked list.
         * class ListNode(var `val`: Int) {
         *     var next: ListNode? = null
         * }
         */
        class ListNode(var `val`: Int) {
            var next: ListNode? = null
        }

        fun reverseList(head: ListNode?): ListNode? {
            var prev: ListNode? = null
            var cur = head
            while (cur != null) {
                val temp = cur.next
                cur.next = prev
                prev = cur
                cur = temp
            }
            return prev
        }
```
```
        //94. Binary Tree Inorder Traversal----------------------------------------------------------------------------
        /**
         * Example:
         * var ti = TreeNode(5)
         * var v = ti.`val`
         * Definition for a binary tree node.
         * class TreeNode(var `val`: Int) {
         *     var left: TreeNode? = null
         *     var right: TreeNode? = null
         * }
         */
        class TreeNode(var `val`: Int) {
            var left: TreeNode? = null
            var right: TreeNode? = null
        }

        fun inorderTraversal(root: TreeNode?): List<Int> {
            val result = mutableListOf<Int>()
            fun rec(root: TreeNode?) {
                if (root == null) return
                rec(root.left)
                result.add(root.`val`)
                rec(root.right)
            }
            rec(root)
            return result
        }
```
```
        //1. Two Sum---------------------------------------------------------------------------------------------------
        fun twoSum(nums: IntArray, target: Int): IntArray {
            val hmap = HashMap<Int, Int>()
            val res = intArrayOf(0, 0)
            for (i in 0..<nums.size) {
                val temp = target - nums[i]
                if (temp in hmap) {
                    res[0] = hmap[temp]!!
                    res[1] = i
                    break
                }
                hmap.put(nums[i], i)
            }
            return res
        }
```
```
        //125. Valid Palindrome-----------------------------------------------------------------------------------------
        fun isPalindrome(s: String): Boolean {
            var left = 0
            var right = s.length - 1
            while (true) {
                while (!s[left].isLetterOrDigit() and (left < right)) {
                    left++
                }
                while (!s[right].isLetterOrDigit() and (left < right)) {
                    right--
                }
                if (left >= right) break
                if ((s[left].lowercase() != s[right].lowercase())) return false
                left++
                right--
            }
            return true
        }
```
```
        //[1,5,3,5,7,2] => [1,5,3,7,2]----------------------------------------------------------------------------------
        fun removeDuplicates2(nums: IntArray): IntArray {
            val hset = HashSet<Int>()
            val result = mutableListOf<Int>()
            for (i in 0..<nums.size) {
                if (nums[i] !in hset) {
                    hset.add(nums[i])
                    result.add(nums[i])
                }
            }
            return result.toIntArray()
        }

```
```
        //21. Merge Two Sorted Lists------------------------------------------------------------------------------------
        /**
         * Example:
         * var li = ListNode(5)
         * var v = li.`val`
         * Definition for singly-linked list.
         * class ListNode(var `val`: Int) {
         *     var next: ListNode? = null
         * }
         */
        fun mergeTwoLists(list1: ListNode?, list2: ListNode?): ListNode? {
            val head = ListNode(0)
            var cur = head
            var cur1 = list1
            var cur2 = list2
            while ((cur1 != null) and (cur2 != null)) {
                if (cur1!!.`val` < cur2!!.`val`) {
                    cur.next = cur1
                    cur1 = cur1.next
                } else {
                    cur.next = cur2
                    cur2 = cur2.next
                }
                cur = cur.next!!
            }
            cur.next = cur1 ?: cur2
            return head.next
        }

```
```
        //100. Same Tree------------------------------------------------------------------------------------------------
        /**
         * Example:
         * var ti = TreeNode(5)
         * var v = ti.`val`
         * Definition for a binary tree node.
         * class TreeNode(var `val`: Int) {
         *     var left: TreeNode? = null
         *     var right: TreeNode? = null
         * }
         */
        fun isSameTree(p: TreeNode?, q: TreeNode?): Boolean {
            var res = true
            fun dfs(p: TreeNode?, q: TreeNode?) {
                if ((p == null) and (q == null)) return
                dfs(p?.left, q?.left)
                if (p?.`val` != q?.`val`) res = false
                dfs(p?.right, q?.right)
            }
            dfs(p, q)
            return res
        }
```
```

        //108. Convert Sorted Array to Binary Search Tree---------------------------------------------------------------
        /**
         * Example:
         * var ti = TreeNode(5)
         * var v = ti.`val`
         * Definition for a binary tree node.
         * class TreeNode(var `val`: Int) {
         *     var left: TreeNode? = null
         *     var right: TreeNode? = null
         * }
         */
        fun sortedArrayToBST(nums: IntArray): TreeNode? {
            if (nums.isEmpty()) return null
            val n = nums.size / 2
            val head = TreeNode(nums[n])
            head.left = sortedArrayToBST(nums.sliceArray(0..<n))
            head.right = sortedArrayToBST(nums.sliceArray(n + 1..<nums.size))
            return head
        }
```
```

        //141. Linked List Cycle---------------------------------------------------------------------------------------
        /**
         * Example:
         * var li = ListNode(5)
         * var v = li.`val`
         * Definition for singly-linked list.
         * class ListNode(var `val`: Int) {
         *     var next: ListNode? = null
         * }
         */
        fun hasCycle(head: ListNode?): Boolean {
            var tortoise = head
            var hare = head
            while ((hare != null) and (hare?.next != null)) {
                tortoise = tortoise?.next
                hare = hare?.next?.next
                if (tortoise == hare) return true
            }
            return false
        }
```
```

        //144. Binary Tree Preorder Traversal--------------------------------------------------------------------------
        /**
         * Example:
         * var ti = TreeNode(5)
         * var v = ti.`val`
         * Definition for a binary tree node.
         * class TreeNode(var `val`: Int) {
         *     var left: TreeNode? = null
         *     var right: TreeNode? = null
         * }
         */
        fun preorderTraversal(root: TreeNode?): List<Int> {
            val result = mutableListOf<Int>()
            fun rec(root: TreeNode?) {
                if (root == null) return
                result.add(root.`val`)
                rec(root.left)
                rec(root.right)
            }
            rec(root)
            return result
        }
```
```
        //1971. Find if Path Exists in Graph----------------------------------------------------------------------------
        fun validPath(n: Int, edges: Array<IntArray>, source: Int, destination: Int): Boolean {
            val graph = mutableMapOf<Int, MutableList<Int>>()
            for (i in 0 until n) {
                graph[i] = mutableListOf()
            }
            for ((u, v) in edges) {
                if (u != v) {
                    graph[u]?.add(v)
                    graph[v]?.add(u)
                }
            }

            fun dfs(graph: MutableMap<Int, MutableList<Int>>, start: Int, end: Int, visited: MutableSet<Int>): Boolean {
                if (start == end) {
                    return true
                }
                if (start in visited) return false
                visited.add(start)
                for (neighbor in graph[start] ?: emptyList()) {
                    if (dfs(graph, neighbor, end, visited)) {
                        return true
                    }

                }
                return false
            }
            return dfs(graph, source, destination, mutableSetOf())
        }

```
```
        //69. Sqrt(x)---------------------------------------------------------------------------------------------------
        fun mySqrt(x: Int): Int {
            var left: Long = 0
            var right: Long = x.toLong()
            while(left <= right){
                val mid = (left+right)/2
                if(mid*mid < x) left = mid + 1
                else if(mid*mid > x) right = mid - 1
                else return mid.toInt()
            }
            return right.toInt()
        }

```
```
        //3043. Find the Length of the Longest Common Prefix------------------------------------------------------------
        fun longestCommonPrefix(arr1: IntArray, arr2: IntArray): Int {
            val prefix = HashSet<String>()
            var res = 0
            arr1.forEach { int ->
                var str = ""
                int.toString().forEach { s ->
                    str += s
                    prefix.add(str)
                }
            }
            arr2.forEach { int ->
                var str = ""
                int.toString().forEach { s ->
                    str += s
                    if (str in prefix){
                        res = max(str.length, res)
                    }
                }

            }
            return res
        }
```
```
        //1710. Maximum Units on a Truck--------------------------------------------------------------------------------
        fun maximumUnits(boxTypes: Array<IntArray>, truckSize: Int): Int {
            boxTypes.sortByDescending{ it[1] }
            var cntUnit = 0
            var cntBox = 0
            boxTypes.forEach{ box ->
                for(i in 0..<box[0]){
                    if(cntBox >= truckSize) return cntUnit
                    cntUnit += box[1]
                    cntBox++
                }
            }
            return cntUnit
        }
```
```
        //11. Container With Most Water
        fun maxArea(height: IntArray): Int {
            var result = 0
            var left = 0
            var right = height.size - 1
            var n = 0
            while(left < right){
                if(height[left] < height[right]){
                    n = height[left]
                    result = max(result, n*(right-left))
                    left++
                }
                else{
                    n = height[right]
                    result = max(result, n*(right-left))
                    right--
                }
            }
            return result
        }
```
```
        //1094. Car Pooling
        fun carPooling(trips: Array<IntArray>, capacity: Int): Boolean {
            val stops = IntArray(1001)
            for((pas, from, to) in trips){
                stops[from] += pas
                stops[to] -= pas
            }
            var pas = 0
            for(i in 0..1000){
                pas += stops[i]
                if(pas > capacity) return false
            }
            return true
        }

    }

}
```
