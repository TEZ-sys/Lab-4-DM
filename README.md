# Lab-4-DM
from collections import defaultdict


class Graph:

    def __init__(self, graph):
        self.graph = graph
        self.ROW = len(graph)

    def searching_algo_BFS(self, s, t, parent):

        visited = [False] * (self.ROW)
        queue = []

        queue.append(s)
        visited[s] = True

        while queue:

            u = queue.pop(0)

            for ind, val in enumerate(self.graph[u]):
                if visited[ind] == False and val > 0:
                    queue.append(ind)
                    visited[ind] = True
                    parent[ind] = u

        return True if visited[t] else False

    def ford_fulkerson(self, source, sink):
        parent = [-1] * (self.ROW)
        max_flow = 0
        chosen_paths = []

        while self.searching_algo_BFS(source, sink, parent):

            path_flow = float("Inf")
            s = sink
            chosen_path = []
            while (s != source):
                path_flow = min(path_flow, self.graph[parent[s]][s])
                chosen_path.append(s)
                s = parent[s]

            chosen_path.append(source)
            chosen_path.reverse()
            chosen_paths.append(chosen_path)

            max_flow += path_flow

            v = sink
            while (v != source):
                u = parent[v]
                self.graph[u][v] -= path_flow
                self.graph[v][u] += path_flow
                v = parent[v]

        return max_flow, chosen_paths

def read_matrix_from_txt(file_path):
    matrix = []
    with open(file_path, 'r') as file:
        lines = file.readlines()
        V=lines[0]
        lines.remove(V)
        for line in lines:
            row = list(map(int, line.strip().split()))
            matrix.append(row)

    return matrix


file_path = 'input.txt'
graph = read_matrix_from_txt(file_path)

g = Graph(graph)

source = 0
sink = len(graph) - 1

max_flow, chosen_paths = g.ford_fulkerson(source, sink)

print("Max Flow: %d " % max_flow)
print("Chosen Paths: ", chosen_paths)
