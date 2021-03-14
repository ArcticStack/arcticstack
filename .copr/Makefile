rpmbuild_src := $(shell rpmbuild --eval '%{_sourcedir}')
rpmbuild_spec := $(shell rpmbuild --eval '%{_specdir}')
specfile := $(rpmbuild_spec)/arcticstack.spec
spec_in := resources/packaging/arcticstack.spec.in

prepare:
	dnf install rpm-build git -y
	mkdir -p $(rpmbuild_src) $(rpmbuild_spec)

VERSION: prepare
	@sh ./resources/packaging/get_version
-include VERSION

$(specfile): $(spec_in) VERSION
	sed 's/@@VERSION@@/$(ARCTICSTACK_VERSION)/g' $(spec_in) > $@

dist: prepare $(specfile)
	git archive HEAD --prefix=arcticstack-$(ARCTICSTACK_VERSION)/ \
		--output=$(rpmbuild_src)/arcticstack-$(ARCTICSTACK_VERSION).tar.gz

srpm: dist
	rpmbuild -bs --define "_srcrpmdir $(outdir)" $(specfile)

rpms: srpm
	rpmbuild -bb --define "_rpmdir $(outdir)" $(specfile)

distclean: VERSION
	rm -f $(rpmbuild_src)/arcticstack-$(ARCTICSTACK_VERSION).tar.gz
	rm -f $(rpmbuild_spec)/arcticstack.spec

.PHONY: dist distclean prepare rpms srpm
