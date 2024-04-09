###################
# BASE IMAGE 
###################
FROM node:18.14.0-alpine as base
RUN echo "Preparing Base Container..." 

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable
RUN pnpm --version
RUN apk add g++ make py3-pip
WORKDIR /usr/src/app

COPY --chown=node:node package.json ./
COPY --chown=node:node pnpm-lock.yaml ./

###################
# Development BUILD 
###################
FROM base as development-build
RUN echo "Building Development Target..." 
ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}

RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --frozen-lockfile
COPY --chown=node:node . .

RUN mkdir -p node_modules/.cache && chmod -R 777 node_modules/.cache
RUN pnpm run build --configuration=development

EXPOSE 4200
CMD ["npx", "ng", "run", "ssrtest:serve-ssr"]