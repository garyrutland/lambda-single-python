ARG VERSION=3.13

# build
FROM python:${VERSION} AS build

ARG DIR

WORKDIR /app

COPY ${DIR}/src /app

RUN pip install --no-cache-dir -r ./requirements.txt

# lambda
FROM public.ecr.aws/lambda/python:${VERSION} AS lambda

ARG VERSION

HEALTHCHECK --interval=1s --timeout=1s --retries=30 \
    CMD curl -I http://localhost:8080

COPY --from=build /app ${LAMBDA_TASK_ROOT}
COPY --from=build /usr/local/lib/python${VERSION}/site-packages ${LAMBDA_TASK_ROOT}

CMD [ "lambda_function.lambda_handler" ]
