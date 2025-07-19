# MyLeetCodeDiary
## Leetcode tasks from my hr
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

5. https://leetcode.com/problems/binary-search/

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




## Когда-то решенные мною задачки

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
