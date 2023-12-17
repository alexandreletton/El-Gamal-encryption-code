# El-Gamal-encryption-code
import random

def mod_exp(base, exponent, modulus):
    
    result = 1
    while exponent > 0:
        if exponent % 2 == 1:
            result = (result * base) % modulus
        base = (base * base) % modulus
        exponent //= 2
    return result

def generate_keypair(p_size=1024):
    
    # Generate a random prime number p
    p = random_prime(p_size)

    # Choose a generator g
    g = random.randint(2, p - 1)

    # Choose a private key a
    a = random.randint(2, p - 2)

    # Calculate the public key h
    h = mod_exp(g, a, p)

    public_key = (p, g, h)
    private_key = a
    return public_key, private_key

def encrypt(message, public_key):
    
    p, g, h = public_key
    r = random.randint(2, p - 2)
    c1 = mod_exp(g, r, p)
    s = mod_exp(h, r, p)
    c2 = (s * message) % p
    return c1, c2

def decrypt(ciphertext, public_key, private_key):
    
    p, _, _ = public_key
    c1, c2 = ciphertext
    s_inv = mod_exp(c1, private_key, p)
    decrypted_message = (c2 * s_inv) % p
    return decrypted_message

def random_prime(bit_size):
    
    while True:
        candidate = random.getrandbits(bit_size)
        if is_prime(candidate):
            return candidate

def is_prime(n, k=5):
    
    if n <= 1 or n % 2 == 0:
        return False

    # Write n - 1 as 2^r * d with d odd
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2

    # Miller-Rabin witnesses
    for _ in range(k):
        a = random.randint(2, n - 2)
        x = mod_exp(a, d, n)
        if x == 1 or x == n - 1:
            continue

        for _ in range(r - 1):
            x = mod_exp(x, 2, n)
            if x == n - 1:
                break
        else:
            return False

    return True






    

   


