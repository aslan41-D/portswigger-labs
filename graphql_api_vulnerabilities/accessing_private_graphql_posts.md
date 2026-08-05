# PortSwigger Lab Solution: Accessing Private GraphQL Posts

**Lab Goal:** Access and read the `postPassword` field for the blog post to obtain the secret password and solve the lab.  
**Tool Used:** Burp Suite Community/Professional (Repeater & Target Tabs)

## Steps to Solve

### 1. Discovering the GraphQL Endpoint
I navigated the target web application and identified that it utilizes a GraphQL API. In Burp Suite's **Target** tab, I confirmed the endpoint exists at `/graphql/v1`. This is the entry point for the vulnerability.

### 2. Enumerating the Schema via Introspection
GraphQL has a built-in feature called **Introspection**, which allows clients to query the API's own schema to understand what data is available. Since this lab has introspection enabled by default, I sent an `__schema` query to the endpoint (or simply attempted to guess valid fields).

By exploring the returned schema, I found that the `getBlogPost` mutation/query actually defines a hidden field named **`postPassword`**, even though it is not displayed in the web app's front-end UI.

### 3. Crafting a Malicious GraphQL Query
Using Burp Suite's **Repeater** tool, I constructed a GraphQL query to retrieve a specific post (in this case, `id: 3`).

Instead of only asking for the public fields (`image`, `title`, `author`, `date`, `paragraphs`), I purposely added the hidden `postPassword` field to the selection set.

**Payload used:**

    ```graphql
      query getBlogPost($id: Int!) {
        getBlogPost(id: $id) {
          image
          title
          author
          date
          paragraphs
          postPassword


        }
      }



<img width="1538" height="775" alt="Ekran Görüntüsü 2026-08-03 21-12-59" src="https://github.com/user-attachments/assets/6a0f2bde-6856-41d6-ad5d-2abd486601bd" />
<img width="1681" height="956" alt="Ekran Görüntüsü 2026-08-03 21-30-00" src="https://github.com/user-attachments/assets/a0d76de4-0d12-44f7-84ab-87b9a4e2dcdd" />
<img width="1681" height="956" alt="Ekran Görüntüsü 2026-08-03 21-39-09" src="https://github.com/user-attachments/assets/ae35b547-6c5e-4e19-a09d-44e3c5a0dc11" />
<img width="1681" height="956" alt="Ekran Görüntüsü 2026-08-03 21-33-25" src="https://github.com/user-attachments/assets/0dc8b757-2f2a-4da0-b7a8-566665e5fe8d" />
