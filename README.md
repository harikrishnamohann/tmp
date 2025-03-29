# LRU
```c
#include <stdio.h>

int main() {
    int i, j, k, min, rs[25], m[10], flag[25], n, f, pf = 0, next = 1, count[10];

    // Input the length of the reference string
    printf("Enter the length of the reference string : ");
    scanf("%d", &n);

    // Input the reference string
    printf("\nEnter the reference string : \n");
    for (i = 0; i < n; i++) {
        scanf("%d", &rs[i]);
        flag[i] = 0; // Initialize flag array to 0 (no page hit initially)
    }

    // Input the number of frames
    printf("\nEnter the no. of frames :");
    scanf("%d", &f);

    // Initialize frames (m) and counter (count) arrays
    for (i = 0; i < f; i++) {
        count[i] = 0; // Initialize counters to 0
        m[i] = -1;    // Initialize frames to -1 (empty)
    }

    printf("\n The page replacement process is : \n");

    // Main loop for page replacement
    for (i = 0; i < n; i++) {
        // Check if the current page (rs[i]) is already in a frame
        for (j = 0; j < f; j++) {
            if (m[j] == rs[i]) {
                flag[i] = 1; // Page hit
                count[j] = next; // Update the counter for the frame
                next++; // Increment the counter
            }
        }

        // If the page is not in any frame (page fault)
        if (flag[i] == 0) {
            // If there are empty frames, fill them
            if (i < f) {
                m[i] = rs[i]; // Put the page into the frame
                count[i] = next; // Update the counter
                next++;
            } else {
                // Find the frame with the least recently used page
                min = 0;
                for (j = 1; j < f; j++) {
                    if (count[min] > count[j]) {
                        min = j;
                    }
                }
                m[min] = rs[i]; // Replace the least recently used page
                count[min] = next; // Update the counter
                next++;
            }
            pf++; // Increment page fault count
        }

        // Print the current state of the frames
        for (j = 0; j < f; j++) {
            printf("%d\t", m[j]);
        }

        // Print "PF no. : pf" if a page fault occurred
        if (flag[i] == 0) {
            printf("PF no. : %d", pf);
        }
        printf("\n");
    }

    // Print the total number of page faults
    printf("\nthe no. of page fault using lru are %d \n", pf);

    return 0;
}
```
#### Explanation:
This C code implements the Least Recently Used (LRU) page replacement algorithm. Here's a breakdown:
 * Initialization:
   * rs[25]: Stores the reference string (sequence of page requests).
   * m[10]: Stores the frames (memory slots).
   * flag[25]: Indicates whether a page hit occurred for each reference string element.
   * n: Length of the reference string.
   * f: Number of frames.
   * pf: Page fault counter.
   * next: Counter used to track the order of page usage.
   * count[10]: Stores the last access time of each frame.
 * Input:
   * The code takes the length of the reference string (n), the reference string itself (rs), and the number of frames (f) as input.
 * LRU Algorithm:
   * The main loop iterates through the reference string.
   * Page Hit Check:
     * It checks if the current page (rs[i]) is already present in any of the frames (m).
     * If it is, flag[i] is set to 1 (page hit), and the corresponding frame's count is updated with the current next value.
   * Page Fault Handling:
     * If the page is not in any frame (flag[i] == 0), a page fault occurs.
     * Empty Frame Check:
       * If there are empty frames (i < f), the page is placed in the first available frame, and the frame's count is updated.
     * LRU Replacement:
       * If there are no empty frames, the LRU algorithm is used to select a frame to replace.
       * The code finds the frame with the smallest count value (least recently used).
       * The current page replaces the page in that frame, and the frame's count is updated.
     * The pf counter is incremented.
   * Output:
     * After each page request, the current state of the frames is printed.
     * If a page fault occurred, "PF no. : pf" is also printed.
   * Finally, the total number of page faults (pf) is printed.
In essence, the code simulates how the LRU page replacement algorithm handles a sequence of page requests, keeping track of page faults and the state of the frames.
