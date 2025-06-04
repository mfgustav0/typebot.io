docker build -t dir/typebot-builder:v1 -f Dockerfile.builder .

docker run --rm \
    --network mysql_mysql \
    --network minio_minio \
    -e "ENCRYPTION_SECRET=hROPBuSRkaIL/CYoLhyp5k46xTPcB7rG" \
    -e "DATABASE_URL=mysql://root:password@mysql:3306/typebot" \
    -e "NEXTAUTH_URL=http://localhost:8081" \
    -e "NEXT_PUBLIC_VIEWER_URL=http://localhost:3001" \
    -e "GITHUB_CLIENT_ID=534b549dd17709a743a2" \
    -e "GITHUB_CLIENT_SECRET=7adb03507504fb1a54422f6c3c697277cfd000a9" \
    -e "S3_ACCESS_KEY=minio" \
    -e "S3_SECRET_KEY=minio123" \
    -e "S3_BUCKET=typebot" \
    -e "S3_PORT=9000" \
    -e "S3_ENDPOINT=localhost" \
    -e "S3_SSL=false" \
    -e "SCOPE=builder" \
    -e "NODE_OPTIONS=--no-node-snapshot" \
    -e "NEXT_PUBLIC_PARTYKIT_HOST=localhost:1999" \
    -p 8081:3000 \
    dir/typebot-builder:v1

npx prisma migrate dev --schema=packages/prisma/postgresql/schema.prisma
