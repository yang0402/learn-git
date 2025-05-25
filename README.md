# learn-git
这是一个学习如何使用git进行团队协作等操作的记录。
## 踩坑日记
### 第一坑
1. Git 需要凭证。
2. Git 发现 GIT_ASKPASS 或 SSH_ASKPASS 环境变量指向了 VS Code 的 askpass.sh。
3. Git 优先调用 askpass.sh。
4. askpass.sh 试图与 VS Code 后台服务通过 Unix socket 通信。
5. 通信失败 (ECONNREFUSED,可能是后台socket未运行)，askpass.sh 无法提供凭证。
6. Git 收到 askpass.sh 失败的信号，但它没有像预期那样退回到 credential.helper store 来提示用户输入，而是直接尝试了清除操作，导致最终认证失败，并继续收到 GitHub 的 401 Unauthorized。
7. 我们通过在 URL 中直接提供凭证的方式，成功地绕过了这个复杂的 askpass 和 credential.helper 的交互问题，直接向 GitHub 提供了有效的认证信息，从而解决了问题。

### 第二坑