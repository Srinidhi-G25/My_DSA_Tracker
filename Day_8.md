Delete all occurrences of a key in DLL

{P.O.V. :  If you really dont get why i make these random commits and uploads, no! its not for the green commit graph marks.I tend to store the code of the sums from my takeuforward sheet that i cant verify and run through leetcode.

Thanks for patiently reading my note!
}

class Solution {
public:
    ListNode* deleteAllOccurrences(ListNode* head, int k) {
        ListNode* curr = head;

        while (curr != nullptr) {
            if (curr->data == k) {
                // Step 1: Save the next and prev nodes
                ListNode* nextNode = curr->next;
                ListNode* prevNode = curr->prev;

                // Step 2: Rewire the previous node's 'next' arrow
                if (prevNode != nullptr) {
                    prevNode->next = nextNode;
                } else {
                    // If there is no prev node, we are deleting the head!
                    head = nextNode;
                }

                // Step 3: Rewire the next node's 'prev' arrow
                if (nextNode != nullptr) {
                    nextNode->prev = prevNode;
                }

                // Step 4: Prevent memory leaks and step forward
                delete curr;
                curr = nextNode;
                
            } else {
                // Not a target? Just keep walking safely down the list.
                curr = curr->next;
            }
        }
        
        return head;
    }
};
/*
Welcome to the world of Doubly Linked Lists (DLL)!

Up until now, you have been dealing with Singly Linked Lists, which are like one-way streets. If you make a mistake and step past a node, you can never go back. A Doubly Linked List gives you a superpower: the prev pointer. You can finally walk backward!

However, with great power comes great responsibility. Because every node has two arrows attached to it (next and prev), every time you delete a node, you have to do twice as much surgical rewiring to fix the chain.

The Intuition: The Weak Link
Imagine a heavy metal chain where every link is hooked to the one in front of it and the one behind it.

If you want to remove a rusty link (your target), you must:

Unhook the rusty link from the link behind it.

Unhook the rusty link from the link in front of it.

Hook the "behind" link and the "in front" link directly to each other.

The Algorithm Step-by-Step
We will walk through the list with a curr (current) pointer. When we find a node that matches the target k, we perform the surgical rewiring.

The Four Big Checks for every deletion:

Save the Next Node: Before we blow up the current node, we must save curr->next in a temporary variable so we don't lose our path forward!

Rewire the Previous Node: If there is a node behind us (curr->prev != nullptr), we tell its next arrow to point to curr->next.

Edge Case: What if curr->prev IS null? That means we are deleting the head! So, we must update the actual head pointer to point to curr->next.

Rewire the Next Node: If there is a node in front of us (curr->next != nullptr), we tell its prev arrow to point back to curr->prev.

Delete and Move On: Physically delete curr from your computer's RAM, and move curr to the saved next node.
*/


Find Pairs with Given Sum in Doubly Linked List



class Solution {
public:
    vector<vector<int>> findPairsWithGivenSum(ListNode *head, int target) {
        vector<vector<int>> ans;
        if (head == nullptr) return ans;

        // Step 1: Find the tail of the Doubly Linked List
        ListNode* left = head;
        ListNode* right = head;
        while (right->next != nullptr) {
            right = right->next;
        }

        // Step 2: The Two-Pointer Squeeze
        // Loop stops if they land on the same node (left == right)
        // or if they cross each other (right->next == left)
        while (left != nullptr && right != nullptr && left != right && right->next != left) {
            
            int sum = left->val + right->val;
            
            if (sum == target) {
                ans.push_back({left->val, right->val});
                left = left->next;   // Move inward
                right = right->prev; // Move inward
            } 
            else if (sum < target) {
                left = left->next;   // We need a bigger sum
            } 
            else {
                right = right->prev; // We need a smaller sum
            }
        }

        return ans;
    }
};
/*
You just unlocked the true power of a Doubly Linked List.

If this were a standard Singly Linked List, finding pairs would be a nightmare of nested loops or using extra memory (like Hash Maps) because you can only move in one direction. But because this list has a prev pointer, it behaves almost exactly like a sorted array.

When you have a sorted sequence and you need to find pairs that add up to a target, the absolute best trick in the book is the Two-Pointer Approach.

The Intuition: The Squeeze
Imagine you are trying to buy exactly $100 worth of parts using one cheap item and one expensive item from a sorted catalog.
You put your left finger on the cheapest item ($10) and your right finger on the most expensive ($120).

$10 + $120 = $130. That's too expensive! Because the catalog is sorted, you know you need a cheaper "expensive" item. You move your right finger down one spot.

$10 + $90 = $100. Perfect! You found a pair.

We are going to do exactly this with our Doubly Linked List. We will put a left pointer at the head, and a right pointer at the tail. Because the list is sorted, we can mathematically "squeeze" them together until they meet in the middle!

The Algorithm Step-by-Step
Find the Tail: The list only gives us the head. So, our first job is to walk a pointer all the way to the end to find the tail.

Set the Pointers: Point left to the head, and right to the tail.

The Squeeze (The Loop): Calculate sum = left->val + right->val.

If sum == target: We found a pair! Add it to our answer array. Move left forward AND move right backward to find the next potential pair.

If sum < target: The sum is too small. We need a bigger number, so we move left forward (left = left->next).

If sum > target: The sum is too big. We need a smaller number, so we move right backward (right = right->prev).

The Stopping Condition: Keep looping until the pointers crash into each other or cross over (left == right or right->next == left).
*/


Remove duplicates from sorted DLL


class Solution {
public:
    ListNode* removeDuplicates(ListNode* head) {
        // Base case: empty list or single node
        if (head == nullptr || head->next == nullptr) return head;

        ListNode* curr = head;

        // Stop when we are at the last node (no 'next' to compare with)
        while (curr != nullptr && curr->next != nullptr) {
            
            if (curr->data == curr->next->data) {
                // Duplicate found! We need to surgically remove curr->next
                ListNode* duplicate = curr->next;
                ListNode* nodeAfterDuplicate = duplicate->next;

                // Rewire the 'next' arrow of curr
                curr->next = nodeAfterDuplicate;

                // Rewire the 'prev' arrow of the node after the duplicate (if it exists)
                if (nodeAfterDuplicate != nullptr) {
                    nodeAfterDuplicate->prev = curr;
                }

                // Free the memory!
                delete duplicate;
                
                // NOTICE: We do NOT move 'curr' forward here. 
                // We stay put to check if the newly attached node is ALSO a duplicate!
            } 
            else {
                // The adjacent node is different. Safe to walk forward.
                curr = curr->next;
            }
        }

        return head;
    }
};

/*
This problem introduces a huge blessing in disguise: the word "sorted".

If the list was jumbled up, removing duplicates would be a nightmare—we would have to use a Hash Set to remember what numbers we’ve seen or use messy nested loops. But because the list is sorted, all identical numbers are forced to stand shoulder-to-shoulder in a line. Because they are clumped together, we can clean up the list in a single, incredibly fast pass.

The Intuition: The "Shoulder-to-Shoulder" Check
Imagine you are walking down a line of people holding numbered signs. You are standing at Node A. You just look at the person immediately to your right (Node B).

Do they have the exact same number as you? If yes, kick them out of the line, and ask the person behind them (Node C) to step up and hold your hand.

Crucial realization: Do not step forward yet! Node C might also be a duplicate. You stay exactly where you are until the person standing next to you holds a different number.

Once they hold a different number, you can safely step forward.

The Algorithm Step-by-Step
Initialize: Start a curr pointer at the head.

The Loop: Keep looping as long as curr != nullptr AND curr->next != nullptr. (We need to ensure there is actually a node next to us to compare against).

The Check: Compare curr->val with curr->next->val.

If they Match (Duplicate Found!): * Save the duplicate node in a temporary pointer so we can delete it later.

Rewire curr->next to skip the duplicate and point to the node after it.

If that node exists, reach over and rewire its prev pointer back to curr.

delete the duplicate to prevent memory leaks.

If they Differ (Safe!):

Move curr forward to the next node (curr = curr->next).
*/