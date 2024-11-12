import hashlib
import time

class Block:
    def __init__(self, index, previous_hash, timestamp, data, proof):
        self.index = index
        self.previous_hash = previous_hash
        self.timestamp = timestamp
        self.data = data
        self.proof = proof
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        block_data = f"{self.index}{self.previous_hash}{self.timestamp}{self.data}{self.proof}"
        return hashlib.sha256(block_data.encode()).hexdigest()

class Blockchain:
    def __init__(self):
        self.chain = [self.create_genesis_block()]

    def create_genesis_block(self):
        return Block(0, "0", time.time(), "Genesis Block", 0)

    def get_latest_block(self):
        return self.chain[-1]

    def add_block(self, new_block):
        new_block.previous_hash = self.get_latest_block().hash
        new_block.hash = new_block.calculate_hash()
        self.chain.append(new_block)

    def proof_of_work(self, block, difficulty):
        block.proof = 0
        while not block.hash.startswith('0' * difficulty):
            block.proof += 1
            block.hash = block.calculate_hash()
        return block.proof

    def add_new_data(self, data, difficulty=2):
        new_block = Block(len(self.chain), self.get_latest_block().hash, time.time(), data, 0)
        self.proof_of_work(new_block, difficulty)
        self.add_block(new_block)

    def is_chain_valid(self):
        for i in range(1, len(self.chain)):
            current_block = self.chain[i]
            previous_block = self.chain[i - 1]
            if current_block.hash != current_block.calculate_hash():
                return False
            if current_block.previous_hash != previous_block.hash:
                return False
        return True

# Example usage:
blockchain = Blockchain()
blockchain.add_new_data("First block after genesis")
blockchain.add_new_data("Second block after genesis")

for block in blockchain.chain:
    print(f"Index: {block.index}, Hash: {block.hash}, Data: {block.data}, Proof: {block.proof}")
print("Blockchain valid:", blockchain.is_chain_valid())


<!---
Ohanian005/Ohanian005 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
