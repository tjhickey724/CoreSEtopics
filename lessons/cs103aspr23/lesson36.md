# Lesson 36
Today we continue learning about coding with nextjs and look at interacting with a server.
We also look at the problem of server-side vs client-side rendering and Search Engine Optimization.

We begin by getting everyone setup with the code from yesterday
and we ask everyone to download the latest version of the [postman app](https://www.postman.com/downloads/)
which will allow us to see how a browser sees our apps!

## APIs


## Interacting with the server
You can create server-side apis by putting components in the /pages/api folder, e.g.
``` javascript
// /pages/api/profile-data.js

// Next.js API route support: https://nextjs.org/docs/api-routes/introduction

export default function handler(req, res) {
    res.status(200).json({
         name: 'Tim Hickey', 
         bio: 'Tim is a CS Professor at Brandeis University' })
  }
```
and then interact with this api hook from the client-side as follows,
after npm installing the swr package.
``` bash
npm install swr
```
which installs the custom hook created by the nextjs team,
and that allows the browser to get data from the server efficiently:
``` javascript
import useSWR from 'swr'

const fetcher = (...args) => fetch(...args).then((res) => res.json())

export default function Profile() {
  const { data, error } = useSWR('/api/profile-data', fetcher)

  if (error) return <div>Failed to load</div>
  if (!data) return <div>Loading...</div>

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.bio}</p>
    </div>
  )
}
```
