# Copyright (c) 2022, 2023 Humanitarian OpenStreetMap Team
# This file is part of GH-Workflows.
#
#     GH-Workflows is free software: you can redistribute it and/or modify
#     it under the terms of the GNU General Public License as published by
#     the Free Software Foundation, either version 3 of the License, or
#     (at your option) any later version.
#
#     GH-Workflows is distributed in the hope that it will be useful,
#     but WITHOUT ANY WARRANTY; without even the implied warranty of
#     MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#     GNU General Public License for more details.
#
#     You should have received a copy of the GNU General Public License
#     along with GH-Workflows.  If not, see <https:#www.gnu.org/licenses/>.
#
ARG MKDOCS_MATERIAL_TAG=9

FROM docker.io/squidfunk/mkdocs-material:${MKDOCS_MATERIAL_TAG} as base
ARG APP_VERSION
ARG COMMIT_REF
ARG MAINTAINER=admin@hotosm.org
LABEL org.hotosm.gh-workflows.app-version="${APP_VERSION}" \
      org.hotosm.gh-workflows.git-commit-ref="${COMMIT_REF:-none}" \
      org.hotosm.gh-workflows.maintainer="${MAINTAINER}"



FROM base as extract-deps
WORKDIR /opt/python
COPY pyproject.toml pdm.lock /opt/python/
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir pdm==2.19.3
RUN pdm export -G docs > requirements.txt



FROM base as runtime
COPY --from=extract-deps \
    /opt/python/requirements.txt /opt/python/
RUN pip install --user --no-warn-script-location \
    --no-cache-dir -r /opt/python/requirements.txt \
    && rm -rf /opt/python
