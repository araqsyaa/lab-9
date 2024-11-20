#include <iostream>
#include <queue>
#include <unordered_map>
#include <vector>
using namespace std;

class Node {
public:
    char ch;
    int freq;
    Node* left;
    Node* right;

    Node(char ch, int freq) : ch(ch), freq(freq), left(nullptr), right(nullptr) {}
    Node(char ch, int freq, Node* left, Node* right) : ch(ch), freq(freq), left(left), right(right) {}
};

class Compare {
public:
    bool operator()(Node* left, Node* right) {
        return left->freq > right->freq;
    }
};

class Huffman {
private:
    Node* root;
    unordered_map<char, string> huffmanCode;

    void buildCodes(Node* node, string str) {
        if (!node) return;

        if (!node->left && !node->right) {
            huffmanCode[node->ch] = str;
        }

        buildCodes(node->left, str + "0");
        buildCodes(node->right, str + "1");
    }

    string decode(Node* node, const string& encoded) {
        string decoded = "";
        Node* current = node;
        for (char bit : encoded) {
            current = (bit == '0') ? current->left : current->right;
            if (!current->left && !current->right) {
                decoded += current->ch;
                current = node;
            }
        }
        return decoded;
    }

public:
    Huffman() : root(nullptr) {}

    void buildTree(const string& text) {
        unordered_map<char, int> freq;
        for (char ch : text) {
            freq[ch]++;
        }

        priority_queue<Node*, vector<Node*>, Compare> pq;

        for (auto& pair : freq) {
            pq.push(new Node(pair.first, pair.second));
        }

        while (pq.size() > 1) {
            Node* left = pq.top(); pq.pop();
            Node* right = pq.top(); pq.pop();
            int sum = left->freq + right->freq;
            pq.push(new Node('\0', sum, left, right));
        }

        root = pq.top();
        buildCodes(root, "");
    }

    void printCodes() {
        for (auto& pair : huffmanCode) {
            cout << pair.first << ": " << pair.second << "\n";
        }
    }

    string encode(const string& text) {
        string encoded = "";
        for (char ch : text) {
            encoded += huffmanCode[ch];
        }
        return encoded;
    }

    string decode(const string& encoded) {
        return decode(root, encoded);
    }
};

int main() {
    string text = "HUFFMAN";
    Huffman huffman;
    huffman.buildTree(text);
    huffman.printCodes();
    string encoded = huffman.encode(text);
    cout << "\nEncoded string:\n" << encoded << "\n";
    string decoded = huffman.decode(encoded);
    cout << "\nDecoded string:\n" << decoded << "\n";
    return 0;
}
