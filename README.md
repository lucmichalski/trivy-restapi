# REST APIs for [Trivy](https://github.com/knqyf263/trivy)

[![CircleCI](https://circleci.com/gh/pottava/trivy-restapi.svg?style=svg)](https://circleci.com/gh/pottava/trivy-restapi)

[![pottava/trivy](http://dockeri.co/image/pottava/trivy)](https://hub.docker.com/r/pottava/trivy/)

Supported tags and respective `Dockerfile` links:  
・latest ([versions/0.1/Dockerfile](https://github.com/pottava/trivy-restapi/blob/master/versions/0.1/Dockerfile))  
・0.1 ([versions/0.1/Dockerfile](https://github.com/pottava/trivy-restapi/blob/master/versions/0.1/Dockerfile))  
・0.1-db ([versions/0.1-db/Dockerfile](https://github.com/pottava/trivy-restapi/blob/master/versions/0.1-db/Dockerfile))  

## Usage

### Run the API server

```bash
$ docker run --name trivy -d --rm -p 9000:9000 \
    -v "${HOME}/Library/Caches/trivy":/root/.cache/trivy \
    pottava/trivy:0.1
```

Then wait about 30 minutes for building the vulnerability database.  
Or

```bash
$ docker run --name trivy -d --rm -p 9000:9000 \
    pottava/trivy:0.1-db
```

### Consume APIs

get repositories ([API spec](https://raw.githubusercontent.com/pottava/trivy-restapi/master/spec.yaml))

```bash
$ curl -s -X GET -H 'Content-Type:application/json' \
  "http://localhost:9000/api/v1/images/python%3A3.4.10-alpine3.9/vulnerabilities" \
  | jq .
{
  "Count": 1,
  "Vulnerabilities": [
    {
      "Description": "ChaCha20-Poly1305 is ...",
      "FixedVersion": "1.1.1b-r1",
      "InstalledVersion": "1.1.1a-r1",
      "PkgName": "openssl",
      "References": [
        "https://www.openssl.org/news/secadv/20190306.txt",
        "..."
      ],
      "Severity": "MEDIUM",
      "Title": "openssl: ChaCha20-Poly1305 with long nonces",
      "VulnerabilityID": "CVE-2019-1543"
    }
  ]
}
$ curl -s -X GET -H 'Content-Type:application/json' \
  "http://localhost:9000/api/v1/images/envoyproxy%2Fenvoy-alpine%3Av1.10.0/vulnerabilities?skip-update=yes" \
  | jq -r ".Count"
1
```
