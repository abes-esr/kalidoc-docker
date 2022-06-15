# TODO: probablement en faire un dépot docker dédié soit
# générique car d'autres application en auront besoin, soit spécifique a kalidoc
# l'avantage d'en faire une spécifique c'est qu'on pourrait aussi y ajouter les
# schéma + échantillon de données spécifique à kalidoc
FROM postgres:14.3

# config de postgresql pour être en Français:
# cf https://github.com/docker-library/docs/blob/master/postgres/README.md#locale-customization
RUN localedef -i fr_FR -c -f UTF-8 -A /usr/share/locale/locale.alias fr_FR.UTF-8
ENV LANG fr_FR.utf8
