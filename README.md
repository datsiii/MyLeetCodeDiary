# MyLeetCodeDiary
## Leetcode tasks from yandex hr

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

    Вторая часть задачки с поиском в повернутом массиве. Нам нужно только определить, есть ли заданное число в массиве. Но теперь проблема: числа в массиве могут повторяться. А на что это влияет? Pivot утерян! Условие nums[left] == nums[mid] не позволяет однозначно определить, какая половина отсортирована.

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
