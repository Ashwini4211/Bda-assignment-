#mapper
import sys

def mapper():
    for line in sys.stdin:
        node, data = line.strip().split("\t")
        distance, friends = eval(data)  # Parse tuple
        print(f"{node}\t({distance}, {friends})")  # Pass node data
        
        if distance != float('inf'):  # Propagate new distances
            for friend in friends:
                print(f"{friend}\t({distance + 1}, [])")

if __name__ == "__main__":
    mapper()
#reducer

import sys

def reducer():
    graph = {}
    for line in sys.stdin:
        node, data = line.strip().split("\t")
        distance, friends = eval(data)
        
        if node not in graph:
            graph[node] = (float('inf'), [])
        
        current_distance, current_friends = graph[node]
        graph[node] = (min(current_distance, distance), current_friends or friends)
    
    for node, (distance, friends) in graph.items():
        print(f"{node}\t({distance}, {friends})")

if __name__ == "__main__":
    reducer()
