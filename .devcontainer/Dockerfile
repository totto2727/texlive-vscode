FROM korosuke613/ubuntu-texlive-ja:latest

ARG ARG_USER_NAME
ARG ARG_USER_UID
ARG ARG_USER_GID
ARG ARG_USER_HOME
ARG ARG_USER_WORKSPACE

ENV USER_NAME=${ARG_USER_NAME}
ENV USER_UID=${ARG_USER_UID}
ENV USER_GID=${ARG_USER_GID}
ENV USER_WORKSPACE=${ARG_USER_WORKSPACE}
ENV USER_HOME=${ARG_USER_HOME}

ENV VOLTA_HOME="$HOME/.volta"
ENV PATH="$VOLTA_HOME/bin:$PATH"

COPY library-scripts/*.sh library-scripts/*.env /tmp/library-scripts/

RUN apt update \
    && export DEBIAN_FRONTEND=noninteractive \
    && /bin/bash /tmp/library-scripts/common-debian.sh false ${USER_NAME} ${USER_UID} ${USER_GID} false true true \
    && apt install -y git gcc cpanminus texlive-extra-utils make \
    && apt autoremove -y \
    && apt clean -y \
    && rm -rf /var/lib/apt/lists/* /tmp/library-scripts \
    && cpanm Log::Log4perl Log::Dispatch::File YAML::Tiny File::HomeDir Unicode::GCString \
    && curl https://get.volta.sh | bash \
    && volta install node@14

USER $USER_NAME
WORKDIR $USER_WORKSPACE

COPY ./.latexmkrc $USER_HOME/.latexmkrc
COPY ./.textlintrc $USER_HOME/.textlintrc

RUN npm init -y \
    && npm i -D prh textlint textlint-rule-preset-ja-technical-writing textlint-rule-preset-jtf-style textlint-rule-prh textlint-plugin-latex2e \
    && npm cache clean --force
