# Choose your containers with care #

* 如果希望 insertion 或 erasure 可以被  roll back （Transactional sematics）就必须使用 node-based container.
* 最小化 iteration, pointer, reference 的 invalidation, 应该使用 node-based container, 因为 在 node-based container 上的 insertion, eraseure 不会造成 iterations, pointers, references 的无效。
