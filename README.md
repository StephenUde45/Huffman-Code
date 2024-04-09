# Huffman Coding in Python

# Input string to be encoded
string = 'testme'

# Creating tree nodes for Huffman encoding
class NodeTree(object):
    def __init__(self, left=None, right=None):
        self.left = left
        self.right = right
    
    # Returns children of the node
    def children(self):
        return (self.left, self.right)
    
    # Returns nodes
    def nodes(self):
        return (self.left, self.right)
    
    # String representation of the node
    def __str__(self):
        return '%s_%s' % (self.left, self.right)

# Function to generate Huffman codes for characters
def huffman_code_tree(node, left=True, binString=''):
    # If the node is a leaf node (character)
    if type(node) is str:
        return {node: binString}
    
    # Recursively traverse the tree to generate Huffman codes
    (l, r) = node.children()
    d = dict()
    d.update(huffman_code_tree(l, True, binString + '0')) # Assign '0' for left child
    d.update(huffman_code_tree(r, False, binString + '1')) # Assign '1' for right child
    return d

# Calculating frequency of characters in the input string
freq = {}
for c in string:
    if c in freq:
        freq[c] += 1
    else:
        freq[c] = 1

# Sorting characters by frequency in descending order
freq = sorted(freq.items(), key=lambda x: x[1], reverse=True)

# Initializing nodes with frequencies
nodes = freq

# Constructing the Huffman tree
while len(nodes) > 1:
    # Extracting two nodes with the lowest frequency
    (key1, c1) = nodes[-1]
    (key2, c2) = nodes[-2]
    nodes = nodes[:-2]
    
    # Creating a new internal node with combined frequency
    node = NodeTree(key1, key2)
    nodes.append((node, c1 + c2))

    # Sorting the nodes by frequency
    nodes = sorted(nodes, key=lambda x: x[1], reverse=True)

# Generating Huffman codes for characters
huffmanCode = huffman_code_tree(nodes[0][0])

# Printing Huffman codes for characters
print(' Char | Huffman code ')
print('----------------------')
for (char, frequency) in freq:
    print(' %-4r |%12s' % (char, huffmanCode[char]))
