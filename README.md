# Java & C++ Algorithm

## BFS

```java
class Vertex {
    int value;
    boolean visited;
    List<Vertex> neighbors;
    Vertex parent;

    public Vertex(int value) {
        this.value = value;
        this.visited = false;
        this.neighbors = new ArrayList<>();
        this.parent = null;
    }

    public boolean isVisited() {
        return this.visited;
    }

    public void setVisited(boolean visited) {
        this.visited = visited;
    }

    public int getValue() {
        return this.value;
    }

    public List<Vertex> getNeighbors() {
        return this.neighbors;
    }

    public Vertex addNeighbor(int value) {
        Vertex neighbor = new Vertex(value);
        this.neighbors.add(neighbor);
        return neighbor;
    }
}

class BFS {
    public static Vertex search(int goal, Vertex root) {
        if (root == null) {
            return null;
        }

        Queue<Vertex> queue = new LinkedList<>();
        root.setVisited(true);
        queue.add(root);

        while (!queue.isEmpty()) {
            Vertex v = queue.remove();

            if (v.getValue() == goal) {
                return v;
            }

            for (Vertex w : v.getNeighbors()) {
                if (!w.isVisited()) {
                    w.setVisited(true);
                    w.parent = v;
                    queue.add(w);
                }
            }
        }

        return null;
    }
}
```
