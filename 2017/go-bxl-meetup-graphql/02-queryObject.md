```go
package main

import (
	"log"

	"net/http"

	"github.com/graphql-go/graphql"
	"github.com/graphql-go/handler"
)

type User struct {
	ApiKey string `json:"api_key"`
	Email  string `json:"email"`
	Id     int    `json:"id"`
}

func main() {

	// Schema
	userType := graphql.NewObject(graphql.ObjectConfig{
		Name: "User",
		Fields: graphql.Fields{
			"id": &graphql.Field{
				Type: graphql.Int,
			},
			"email": &graphql.Field{
				Type: graphql.String,
			},
			"api_key": &graphql.Field{
				Type: graphql.String,
			},
		},
	})

	fields := graphql.Fields{
		"user": &graphql.Field{
			Type:        userType,
			Description: "User resources",
			Args: graphql.FieldConfigArgument{
				"key": &graphql.ArgumentConfig{
					Type: &graphql.NonNull{
						OfType: graphql.String,
					},
					Description: "The user api key",
				},
			},
			Resolve: func(p graphql.ResolveParams) (interface{}, error) {
				key := p.Args["key"].(string)
				return User{ApiKey: key, Id: 1, Email: "john.doo@text.me"}, nil
			},
		},
	}
	rootQuery := graphql.ObjectConfig{Name: "RootQuery", Fields: fields}
	schemaConfig := graphql.SchemaConfig{Query: graphql.NewObject(rootQuery)}
	schema, err := graphql.NewSchema(schemaConfig)
	if err != nil {
		log.Fatalf("failed to create new schema, error: %v", err)
	}

	http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
		writer.Write([]byte("Hello world API"))
	})

	h := handler.New(&handler.Config{
		Schema:   &schema,
		Pretty:   true,
		GraphiQL: true,
	})

	http.Handle("/graphql", h)

	if err := http.ListenAndServe(":3000", nil); err != nil {
		log.Fatalln("Error", err)
	}
}
```
