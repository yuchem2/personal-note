```jsx
class UnionFind {
  constructor(n) {
    this.parent = Array.from({ length: n + 1 }, (_, i) => i);
    this.rank = new Array(n + 1).fill(0);
    this.numSets = n; // 현재 분리된 집합의 개수
  }

  find(i) {
    if (this.parent[i] === i) return i;
    // Path Compression: 부모를 루트로 바로 연결해 효율성 극대화
    return this.parent[i] = this.find(this.parent[i]);
  }

  union(i, j) {
    let rootI = this.find(i);
    let rootJ = this.find(j);

    if (rootI !== rootJ) {
      // Union by Rank: 낮은 트리를 높은 트리 밑에 붙임
      if (this.rank[rootI] < this.rank[rootJ]) {
        this.parent[rootI] = rootJ;
      } else if (this.rank[rootI] > this.rank[rootJ]) {
        this.parent[rootJ] = rootI;
      } else {
        this.parent[rootI] = rootJ;
        this.rank[rootJ]++;
      }
      this.numSets--; // 집합이 합쳐질 때마다 개수 감소
      return true;
    }
    return false;
  }
}
```
