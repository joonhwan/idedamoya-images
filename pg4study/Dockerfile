# from : https://stackoverflow.com/a/72384475/884268
FROM postgres:14.5 AS runtime_init
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=postgres
ENV POSTGRES_HOST_AUTH_METHOD=trust
# sample database
COPY ./01-create-db.sql /docker-entrypoint-initdb.d/
COPY ./02-northwind.sql /docker-entrypoint-initdb.d/
COPY ./03-chinook.sql /docker-entrypoint-initdb.d/

FROM runtime_init AS buildtime_init_builder
RUN echo "exit 0" > /docker-entrypoint-initdb.d/100-exit_before_boot.sh
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=postgres
ENV PGDATA=/pgdata
RUN docker-entrypoint.sh postgres

FROM postgres:14.5 AS buildtime_init
LABEL org.opencontainers.image.source https://github.com/joonhwan/igedamoya-images
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=postgres
ENV POSTGRES_HOST_AUTH_METHOD=trust
ENV PGDATA=/pgdata
COPY --chown=postgres:postgres --from=buildtime_init_builder /pgdata /pgdata
