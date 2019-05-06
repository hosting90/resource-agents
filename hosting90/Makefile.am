# Makefile.am for OCF RAs
#
# Author: Sun Jing Dong
# Copyright (C) 2004 IBM
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#
MAINTAINERCLEANFILES = Makefile.in

EXTRA_DIST		= $(ocf_SCRIPTS) $(ocfcommon_DATA) \
			  $(common_DATA) $(hb_DATA) $(dtd_DATA) \
			  README

AM_CPPFLAGS		= -I$(top_srcdir)/include -I$(top_srcdir)/linux-ha

halibdir		= $(libexecdir)/heartbeat

ocfdir		        = $(OCF_RA_DIR_PREFIX)/hosting90

ocf_SCRIPTS	      = dmsetup_error \
                    haproxy-backend-server \
                    mysql-query \
                    oneshot

ocfcommondir		= $(OCF_LIB_DIR_PREFIX)/heartbeat

check: $(ocf_SCRIPTS:=.check)

%.check: %
	OCF_ROOT=$(abs_srcdir) OCF_FUNCTIONS_DIR=$(abs_srcdir) ./$< meta-data | xmllint --path $(abs_srcdir) --noout --relaxng $(abs_srcdir)/metadata.rng -

do_spellcheck = printf '[%s]\n' "$(agent)"; \
                OCF_ROOT=$(abs_srcdir) OCF_FUNCTIONS_DIR=$(abs_srcdir) \
                ./$(agent) meta-data 2>/dev/null \
                | xsltproc $(top_srcdir)/make/extract_text.xsl - \
                | aspell pipe list -d en_US --ignore-case \
                  --home-dir=$(top_srcdir)/make -p spellcheck-ignore \
                | sed -n 's|^&\([^:]*\):.*|\1|p';
spellcheck:
	@$(foreach agent,$(ocf_SCRIPTS), $(do_spellcheck))
