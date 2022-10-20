FROM grokzen/redis-cluster:latest

LABEL maintainer="hareku <hareku908@gmail.com>"

##########################################################################
# Set Timezone
###########################################################################

ARG TZ=UTC
ENV TZ ${TZ}

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone