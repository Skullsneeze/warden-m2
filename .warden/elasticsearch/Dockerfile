ARG ELASTICSEARCH_VERSION

FROM elasticsearch:${ELASTICSEARCH_VERSION}

RUN bin/elasticsearch-plugin install analysis-phonetic
RUN bin/elasticsearch-plugin install analysis-icu