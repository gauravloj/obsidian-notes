- Identify graphql: send `query{__typename}`
- Identify endpoint, common endpoints
	- /graphql
	- /api
	- /api/graphql
	- /graphql/api
	- /graphql/graphql

- Discover schema:
	- Use visualizer: http://nathanrandal.com/graphql-visualizer/
	- Graphql query: 
```json
query IntrospectionQuery {
  __schema {
    queryType {
      name
    }
    mutationType {
      name
    }
    subscriptionType {
      name
    }
    types {
      ...FullType
    }
    directives {
      name
      description
      args {
        ...InputValue
      }
      onOperation
      onFragment
      onField
    }
  }
}

fragment FullType on __Type {
  kind
  name
  description
  fields(includeDeprecated: true) {
    name
    description
    args {
      ...InputValue
    }
    type {
      ...TypeRef
    }
    isDeprecated
    deprecationReason
  }
  inputFields {
    ...InputValue
  }
  interfaces {
    ...TypeRef
  }
  enumValues(includeDeprecated: true) {
    name
    description
    isDeprecated
    deprecationReason
  }
  possibleTypes {
    ...TypeRef
  }
}

fragment InputValue on __InputValue {
  name
  description
  type {
    ...TypeRef
  }
  defaultValue
}

fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
      }
    }
  }
}
```

	- Full introspection: [https://portswigger.net/web-security/graphql](https://portswigger.net/web-security/graphql)
	- https://github.com/nikitastupin/clairvoyance 
- 
- 