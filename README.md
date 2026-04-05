# Quantum Browser
#### Video Demo: [YOUTUBE URL HERE]
#### Description:

**Quantum Browser** is a minimalist, privacy-focused web browser built from the ground up using **Electron**, **Node.js**, and **Vanilla JavaScript**. While many modern browsers are known for being resource-heavy and feature-bloated, Quantum was designed as a technical challenge to bridge the gap between high-level web technologies and the fundamental, low-level data structures introduced in CS50x—specifically those from Week 5 (Data Structures).

The core philosophy of this project was to avoid relying solely on high-level abstractions for internal logic. Instead of using standard JavaScript arrays for every feature, I implemented custom data structures to handle tab management, history, and search suggestions. This ensures that the browser operates with optimized time complexity, applying theoretical concepts like $O(1)$ and $O(\log n)$ to a real-world software architecture.

---

### Project Structure and File Descriptions

To maintain a clean and modular codebase, the logic is separated into several key files, each with a specific responsibility:

* **`main.js`**: This is the entry point of the application. It manages the Electron lifecycle, creates the native OS windows, and handles inter-process communication (IPC). It also contains the "Network Interceptor" logic which uses Hash Tables to block tracking domains.
* **`index.html`**: Defines the primary UI layout, including the URL bar, navigation controls, and the `webview` tag used to sandbox web content.
* **`renderer.js`**: Acting as the "central nervous system," this file coordinates between the user's interactions in the UI and the underlying data structures.
* **`tabManager.js`**: Contains the **Doubly Linked List** implementation. This file defines the `TabNode` and `TabList` classes that manage the creation, deletion, and traversal of browser tabs.
* **`trie.js`**: Implements the **Prefix Tree** (Trie) logic. It stores the user's browsing history and provides instant autocomplete suggestions as the user types in the URL bar.
* **`database.js`**: Manages the **Binary Search Tree (BST)** for bookmarks. It handles the logic for inserting, searching, and deleting bookmarks while maintaining alphabetical order.
* **`style.css` & `settings.css`**: These files define the "Quantum Aesthetic"—a dark-mode, minimalist interface designed to reduce visual noise.
* **`package.json`**: Defines the project dependencies and the environment configuration.

---

### Technical Deep Dive: Data Structures in Action

The name "Quantum" refers to the precision of the underlying structures. Here is how I applied specific CS50 lessons:

#### 1. Doubly Linked List for Tab Management
In most browsers, tabs are highly dynamic. Using a standard array would mean that closing a tab at the beginning of the list would force every subsequent tab to shift its index in memory, an $O(n)$ operation. By implementing a **Doubly Linked List**, I ensured that removing any node (tab) only requires updating the `prev` and `next` pointers of its neighbors, resulting in a true $O(1)$ constant time operation.

#### 2. Prefix Tree (Trie) for URL Autocomplete
Efficiency in the URL bar is critical for a good user experience. I built a Trie where each node represents a character. This allows the browser to find suggestions in $O(L)$ time, where $L$ is the length of the string being typed. This remains fast even if the user has thousands of visited sites in their history.

#### 3. Binary Search Tree (BST) for Bookmarks
Bookmarks are stored in a BST to allow for $O(\log n)$ search times. I spent significant time implementing the `remove` method for the BST, which was one of the most challenging parts of the logic—specifically handling the "two children" case by dynamically discovering the in-order successor to maintain the tree's alphabetical integrity.

#### 4. Hash Tables for Privacy (Ad-Blocking)
The ad-blocking engine uses a **Set (Hash Table)** of known tracker domains. Before any network request is sent by the engine, Quantum checks the destination domain against this table. Thanks to the $O(1)$ average-case lookup time of hash tables, the browser can block spyware packets without any perceptible lag in page loading.

---

### Design Choices and Challenges

One of the most significant design hurdles was the **Chromium Sandbox**. For security reasons, code running inside a website cannot talk directly to the code running the browser UI. 

**The History Problem:** I wanted users to be able to delete history items from an interactive "Library" page. However, since this page runs inside a `webview`, it cannot access the browser's memory directly. 
* **The Solution:** I engineered a "Secure Message Bus." When a user clicks "Delete" on the history page, it emits a specific string to the `console.log`. The `renderer.js` listens to the webview's console chatter, catches the specific command, validates the ID, and then modifies the **LIFO Stack** in the main memory. This maintains a strict security "Air Gap" while providing a native-feeling experience.

Another trade-off involved the **Bookmarks Bar**. While the BST handles the data, users expect to drag and drop icons. Since a BST enforces a specific order, I had to separate the "Data Layer" from the "View Layer." The BST remains the source of truth for searching, while a secondary `localStorage` mapping handles the visual coordinates of the icons.

---

### Installation and Usage

To run Quantum Browser locally, you need **Node.js** and **npm** installed.

1.  Clone the repository:
    ```bash
    git clone https://github.com/Nordwyn/Quantum-Browser.git
    ```
2.  Navigate to the directory:
    ```bash
    cd Quantum-Browser
    ```
3.  Install dependencies:
    ```bash
    npm install
    ```
4.  Launch the browser:
    ```bash
    npm start
    ```

---

### Academic Honesty & AI Disclosure

In accordance with the CS50x Academic Honesty policy, I would like to disclose the use of AI tools (specifically Google Gemini) during the development of this project:

1.  **Documentation:** AI was used to help draft, translate, and refine the technical descriptions in this `README.md` and to help structure the video demonstration script.
2.  **Problem Solving:** AI was consulted to brainstorm solutions for the **Chromium Sandbox limitation**. The "Secure Message Bus" architecture was developed with the assistance of AI to ensure a secure and functional implementation.
3.  **Code Optimization & Efficiency:** AI was used to refactor several core functions to improve execution speed and memory management. Specifically, it helped optimize the traversal logic in the **Trie** and the recursive methods within the **Binary Search Tree** to ensure they were as efficient as possible.
4.  **Code Refinement:** AI helped identify and fix complex edge cases in the BST deletion logic, particularly regarding the discovery of the in-order successor.

While AI assisted in optimizing and documenting the code, the core architecture, the manual implementation of all data structures, and the final integration of the software remain my own original work.

**This was CS50!**
