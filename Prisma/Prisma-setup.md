# Prisma Setup

1. Install Node.js and Database (eg: SQLite)
2. `npm init -y` `npm i --save-dev prisma typescript ts-node @types/node nodemon` — Install dependencies
3. Create `tsconfig.json` file:

```json
{
	"compilerOptions": {
		"sourceMap": true,
		"outDir": "dist",
		"strict": true,
		"lib": ["esnext"],
		"esModuleInterop": true
	}
}
```

1. `npx prisma init --datasource-provider mysql` — Initialize Prisma project
2. All the schema of the database will be written in a file named `schema.prisma`. The schema file is written in Prisma Schema Language (PSL).
3. You can format the Schema written in PSL using the command `npx prisma format`
4. Every time you make changes to your `schema.prisma` file, then you have to apply those changes to your database, for that purpose you have to run the command `npx prisma migrate dev --name testing`. The last argument in the command is the name which you can optionally provide for each migration. The argument `dev` specifies that this if for development environment.
5. We also need a Prisma client for interacting with our database programmatically using Prisma `npm install @prisma/client`
6. For manually regenerating the client code you can use the command `npx prisma generate`
7. Then we can create a new file in the root of the project named for example as `script.ts`, in which we can write our code for interacting with our database. Boilerplate code will look something like this: 

```tsx
import {PrismaClient} from "@prisma/client";
const prisma = new PrismaClient();

async function main() {
	// write code here for interacting with your database programmatically
}

main()
	.catch(e => {
		console.error(e.message)
	})
	.finally(e => {
		// Destroy the connection after the file has completed execution
		// otherwise prisma will create a new connection every time the file is executed
		await prisma.$disconnect()
	})
```

1. To execute the above file, use the command `node script.ts`