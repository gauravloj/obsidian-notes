- JWT: base64 string separated by dots `.` : `<header>.<payload>.<signature>`
- auto recon: https://github.com/ticarpi/jwt_tool.git
- 
- decode JWT 
```sh
jwtd() {
    if [[ -x $(command -v jq) ]]; then
         jq -R 'split(".") | .[0],.[1] | @base64d | fromjson' <<< "${1}"
         echo "Signature: $(echo "${1}" | awk -F'.' '{print $3}')"
    else
	    echo "$1" | awk -F'.' '{print $1}' | base64 -d
	    echo "$1" | awk -F'.' '{print $1}' | base64 -d
	    echo "Signature: $(echo "${1}" | awk -F'.' '{print $3}')"
    fi
}
```

## Vulnerabilities
- No Signature verification: Update the claims and get the info
- Downgrade the algorithm to `None` in header
- Weak symmetric secrets: using weak secrets to generate the JWT signature
	- Save the JWT to a text file called jwt.txt.
	- Download a common JWT secret list. Eg. `wget https://raw.githubusercontent.com/wallarm/jwt-secrets/master/jwt.secrets.list` 
	- Use Hashcat to crack the secret using `hashcat -m 16500 -a 0 jwt.txt jwt.secrets.list`
- Algorithm Confusion: Downgrading encryption algorithm from Asymmetric to symmetric can cause backend to use the public key for signature verification
- Cross-service relay attacks: Issue with unverified audience claims. A user who is admin on one service can utilize admin privilege on other service on which he doesn't have admin privilege. Both services are authenticated by same authentication server.
- Bash scripts to encode and decode JWTs for HS256

```sh
#!/usr/bin/env bash

#
# JWT Encoder Bash Script
#

secret='SOME SECRET'

# Static header fields.
header='{
	"typ": "JWT",
	"alg": "HS256",
	"kid": "0001",
	"iss": "Bash JWT Generator"
}'

# Use jq to set the dynamic `iat` and `exp`
# fields on the header using the current time.
# `iat` is set to now, and `exp` is now + 1 second.
header=$(
	echo "${header}" | jq --arg time_str "$(date +%s)" \
	'
	($time_str | tonumber) as $time_num
	| .iat=$time_num
	| .exp=($time_num + 1)
	'
)
payload='{
	"Id": 1,
	"Name": "Hello, world!"
}'

base64_encode()
{
	declare input=${1:-$(</dev/stdin)}
	# Use `tr` to URL encode the output from base64.
	printf '%s' "${input}" | base64 | tr -d '=' | tr '/+' '_-' | tr -d '\n'
}

json() {
	declare input=${1:-$(</dev/stdin)}
	printf '%s' "${input}" | jq -c .
}

hmacsha256_sign()
{
	declare input=${1:-$(</dev/stdin)}
	printf '%s' "${input}" | openssl dgst -binary -sha256 -hmac "${secret}"
}

header_base64=$(echo "${header}" | json | base64_encode)
payload_base64=$(echo "${payload}" | json | base64_encode)

header_payload=$(echo "${header_base64}.${payload_base64}")
signature=$(echo "${header_payload}" | hmacsha256_sign | base64_encode)

echo "${header_payload}.${signature}"
```

- Decoder script

```sh
#!/usr/bin/env bash

#
# JWT Decoder Bash Script
#

secret='SOME SECRET'

base64_encode()
{
	declare input=${1:-$(</dev/stdin)}
	# Use `tr` to URL encode the output from base64.
	printf '%s' "${input}" | base64 | tr -d '=' | tr '/+' '_-' | tr -d '\n'
}

base64_decode()
{
	declare input=${1:-$(</dev/stdin)}
	# A standard base64 string should always be `n % 4 == 0`. We made the base64
	# string URL safe when we created the JWT, which meant removing the `=`
	# signs that are there for padding. Now we must add them back to get the
	# proper length.
	remainder=$((${#input} % 4));
	if [ $remainder -eq 1 ];
	then
		>2& echo "fatal error. base64 string is unexepcted length"
	elif [[ $remainder -eq 2 || $remainder -eq 3 ]];
	then
		input="${input}$(for i in `seq $((4 - $remainder))`; do printf =; done)"
	fi
	printf '%s' "${input}" | base64 --decode
}

verify_signature()
{
	declare header_and_payload=${1}
	expected=$(echo "${header_and_payload}" | hmacsha256_encode | base64_encode)
	actual=${2}

	if [ "${expected}" = "${actual}" ]
	then
		echo "Signature is valid"
	else
		echo "Signature is NOT valid"
	fi
}

hmacsha256_encode()
{
	declare input=${1:-$(</dev/stdin)}
	printf '%s' "${input}" | openssl dgst -binary -sha256 -hmac "${secret}"
}

# Read the token from stdin
declare token=${1:-$(</dev/stdin)};

IFS='.' read -ra pieces <<< "$token"

declare header=${pieces[0]}
declare payload=${pieces[1]}
declare signature=${pieces[2]}

echo "Header"
echo "${header}" | base64_decode | jq
echo "Payload"
echo "${payload}" | base64_decode | jq

verify_signature "${header}.${payload}" "${signature}"
```