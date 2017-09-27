# GraphQL Exemples

The examples are valid on [graphqlhub](https://www.graphqlhub.com)

## Fields 
```graphql
{
  github{
    user(username:"alexandreroba"){
      id
      login
      company
      repos{
        id
        name
      }
    }
  }
}
```

## Queries
```graphql
query getGitHubUser{
  github{
    user(username:"alexandreroba"){
      id
      login
      company
      repos{
        id
        name
      }
    }
  }
}
```

## Variables
```graphql
query getGitHubUser($userName:String!){
  github{
    user(username:$userName){
      id
      login
      company
      repos{
        id
        name
      }
    }
  }
}

{"userName": "alexandreroba"}
```

## Directives
```graphql
query getGitHubUser($userName:String!,$includeRepo:Boolean!){
  github{
    user(username:$userName){
      id
      login
      company
      repos@include(if: $includeRepo){
        id
        name
      }
    }
  }
}

{"userName": "alexandreroba","includeRepo":false}
```

## Alias
```graphql
query getTwoUsers($userName1:String!,$userName2:String!){
  github{
    user1:user(username:$userName1){
      id
      username: login
      company
    }
    user2:user(username:$userName2){
      id
      username: login
      company
    }
  }
}

{"userName1": "alexandreroba","userName2":"suancarloj"}
```

## Fragments
```graphql
 query getTwoUsers($userName1:String!,$userName2:String!){
   github{
     user1:user(username:$userName1){
       ...userInfo
     }
     user2:user(username:$userName2){
       ...userInfo
     }
   }
 }

fragment userInfo on GithubUser{
  id
  username:login
  company
}

{"userName1": "alexandreroba","userName2":"suancarloj"}
```






