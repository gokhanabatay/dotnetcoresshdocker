
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 As builder

WORKDIR /app

COPY *.csproj ./

RUN dotnet restore

COPY . ./

RUN dotnet publish -c Release -o out

# Stage 2
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1

RUN apt-get update && apt-get install -y supervisor && apt-get install -y openssh-server && echo "root:Docker!" | chpasswd 

RUN mkdir -p /var/log/supervisor /run/sshd

COPY sshd_config /etc/ssh/
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

WORKDIR /app
COPY --from=builder /app/out .

EXPOSE 80 2222

ENTRYPOINT ["/usr/bin/supervisord"]