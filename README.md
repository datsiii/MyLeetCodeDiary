# MyLeetCodeDiary
Leetcode tasks from yandex hr
2. https://leetcode.com/problems/linked-list-cycle/
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
