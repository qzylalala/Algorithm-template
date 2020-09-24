## Morris 中序遍历

### 算法模板

```c++
// 数据结构
/* struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
// 迭代过程
		TreeNode *cur = root, *pre = nullptr;
		while (cur) {
            if (!cur->left) {
                update(cur->val);
                cur = cur->right;
                continue;
            }
            // 找到 cur 的前驱
            pre = cur->left;
            while (pre->right && pre->right != cur) pre = pre->right;
            // 第一次访问 cur 的前驱, 建立线索
            if (!pre->right) {
                pre->right = cur;
                cur = cur->left;
            } else {      //  第二次访问 cur 的前驱, 说明已经访问过左子树, 还原结构
                pre->right = nullptr;
                update(cur->val);
                cur = cur->right;
            }
        }
```

