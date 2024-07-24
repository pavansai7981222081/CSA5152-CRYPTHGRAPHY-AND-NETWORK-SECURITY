import numpy as np

def text_to_numbers(text):
    
    alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    return [alphabet.index(c) for c in text.upper() if c in alphabet]

def numbers_to_text(numbers):
    
    alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    return ''.join(alphabet[num % 26] for num in numbers)

def mod_inverse(matrix, mod):
   
    det = int(np.round(np.linalg.det(matrix)))  
    det_inv = pow(det, -1, mod)  
    u
    matrix_adj = np.round(np.linalg.inv(matrix) * np.linalg.det(matrix)).astype(int) % mod
    matrix_inv = (det_inv * matrix_adj) % mod
    
    return matrix_inv

def encrypt_row_vector(plaintext_row, key_matrix):
    
    
    plaintext_row = np.array(plaintext_row)
    key_matrix = np.array(key_matrix)
    
    
    encrypted_block = np.dot(plaintext_row, key_matrix) % 26
    
    
    return encrypted_block.astype(int).tolist()

def decrypt_row_vector(encrypted_row, key_matrix_inv):
    
    
    encrypted_row = np.array(encrypted_row)
    key_matrix_inv = np.array(key_matrix_inv)
    
    
    decrypted_block = np.dot(encrypted_row, key_matrix_inv) % 26
    
    
    return decrypted_block.astype(int).tolist()

def main():
    
    plaintext = input("Enter plaintext: ").upper()  
    
    
    key_matrix = np.array([
        [17, 17, 5],
        [21, 18, 21],
        [2, 2, 19]
    ])
    
    
    mod = 26
    key_matrix_inv = mod_inverse(key_matrix, mod)
    
    
    plaintext_numbers = text_to_numbers(plaintext)
    
    
    size = key_matrix.shape[0]
    if len(plaintext_numbers) % size != 0:
        plaintext_numbers += [0] * (size - len(plaintext_numbers) % size)  
    
    
    encrypted_numbers = []
    for i in range(0, len(plaintext_numbers), size):
        block = plaintext_numbers[i:i + size]  
        encrypted_block = encrypt_row_vector(block, key_matrix)
        encrypted_numbers.extend(encrypted_block)
    
    
    encrypted_text = numbers_to_text(encrypted_numbers)
    
    
    decrypted_numbers = []
    for i in range(0, len(encrypted_numbers), size):
        block = encrypted_numbers[i:i + size]  
        decrypted_block = decrypt_row_vector(block, key_matrix_inv)
        decrypted_numbers.extend(decrypted_block)
    
    
    decrypted_text = numbers_to_text(decrypted_numbers)
    
    
    print(f"Key matrix:\n{key_matrix}")
    print(f"Inverse key matrix:\n{key_matrix_inv}")
    print(f"Original plaintext: {plaintext}")
    print(f"Encrypted text: {encrypted_text}")
    print(f"Decrypted text: {decrypted_text}")

if __name__ == "__main__":
      main()
