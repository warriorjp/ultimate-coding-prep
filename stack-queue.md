Find kth max element:
Whenever you put numbers inside a priority queue, it automatically keeps the smallest number at the top.

nums = [3,2,1,5,6,4]
k = 2

import java.util.PriorityQueue;

class Solution {
    public int findKthLargest(int[] nums, int k) {

        PriorityQueue<Integer> pq = new PriorityQueue<>();

        for (int num : nums) {
            pq.offer(num);

            if (pq.size() > k) {
                pq.poll();
            }
        }

        return pq.peek();
    }
}
