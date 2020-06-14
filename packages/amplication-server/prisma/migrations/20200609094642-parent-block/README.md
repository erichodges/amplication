# Migration `20200609094642-parent-block`

This migration has been generated by Yuval Hazaz at 6/9/2020, 9:46:42 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "public"."Block" ADD COLUMN "parentBlockId" text   ;

ALTER TABLE "public"."Block" ADD FOREIGN KEY ("parentBlockId")REFERENCES "public"."Block"("id") ON DELETE SET NULL  ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200604151640-json..20200609094642-parent-block
--- datamodel.dml
+++ datamodel.dml
@@ -1,16 +1,16 @@
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url      = env("POSTGRESQL_URL")
   enabled  = true
 }
 generator client {
   provider = "prisma-client-js"
 }
 generator typegraphql {
-  provider = "node_modules/typegraphql-prisma-nestjs/generator.js"
+  provider = "node ./node_modules/typegraphql-prisma/generator.js"
   output   = "./dal"
 }
 model Account {
@@ -162,14 +162,17 @@
   createdAt     DateTime       @default(now())
   updatedAt     DateTime       @updatedAt
   app           App            @relation(fields: [appId], references: [id])
   appId         String
+  parentBlock   Block?         @relation(fields: [parentBlockId], references: [id])
+  parentBlockId String?
   blockType     EnumBlockType
   name          String
   description   String?
   blockVersions BlockVersion[]
   @@unique([appId, name])
+  Block Block[] @relation("BlockToBlock")
 }
 enum EnumBlockType {
   appSettings
```

