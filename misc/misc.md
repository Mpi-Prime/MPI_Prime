# Divers

## Rapports d'épreuves orales
* Mines Telecom :
   * [Rapport Mines-Telecom 2024 (Olivier)](/misc/rapport_mines_tel.pdf)


## Templates LateX
### Template d'Olivier - Documents classiques
* [Header principal](/misc/header.tex)
* [Fichier macros](/misc/macros.tex)
* [Fichier macros lettres](letterfonts.tex)

* Exemple de fichier main (petit extrait) :
```tex

\documentclass[8pt, a4paper]{article}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%% PACKAGES %%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


%% ATTENTION À BIEN UPDATE LES CHEMINS
\usepackage{import}
\import{/Users/sacss/Documents/LateX/}{header}
\import{/Users/sacss/Documents/LateX/}{letterfonts}
\import{/Users/sacss/Documents/LateX/}{macros}


\begin{document}

%% Page de couverture
begin{center}
		\color{marine_blue}
		\large \textbf{MPI* Maths} \\
		\LARGE \textbf{Programme de khôlles} \vskip 0.1cm 
		\normalsize Semaine 23
	\end{center}
    \vspace*{\fill}


  % Illustration milieu de couverture
    \begin{center}
		\includegraphics[scale=0.7]{meme_cover_repl.png}
	\end{center}

  %% Bas de page 
	\vspace*{\fill}
    \begin{center}
		\large Olivier Caffier \vskip 0.2cm
		\href{mailto:oliviercaffier.contact@gmail.com}{\includegraphics[scale=0.018]{/Users/sacss/Documents/LateX/gmail.jpeg}} 
		\href{https://github.com/Mpi-Prime/MPI_Prime}{\includegraphics[scale=0.03]{/Users/sacss/Documents/LateX/github.png}}
		\\
 			\rule{0.9\linewidth}{1pt} 
 		\end{center}
		\newpage


\dfn{- Minimum local et global}{
				Soient $E$ un $\RR$ e.v.n de dim. finie, $U \subset E$ un ouvert, $f : U \to \RR$ et $a \in U$.
				\begin{itemize}
					\item[$\bullet$] On dit que $f$ admet un \emph{minimum global} en $a$ si : \[\forall x \in U, f(x) \geq f(a)\]
					\item[$\bullet$] On dit que $f$ admet un \emph{minimum local} en $a$ si : \[\exists r > 0, \forall x \in B_f(a,r), f(x) \geq f(a)\] 
				\end{itemize}
			}


			\prop{}{
				On considère $E = \RR^n$ muni de sa structure euclidienne canonique, $U \subset E$ un ouvert, $f : U \to \RR$ application $\mathcal{C}^2$ sur $U$ et $a \in U$.
				\\ Alors, 
				\begin{center}
					$f$ admet un minimum (local ou global) en $a$ $\implies \begin{cases}
						a \text{ est un point critique de } f \\
						H_f(a) \in S_n^+(\RR)
					\end{cases}$
				\end{center}
			}



\end{document}
```

### Ressources TIPE 
* Olivier :
    * TIPE 2025 - Problème du transport optimal : au cœur de l'animation météorologique : [Page GitHub](https://github.com/Sacss-dev/TIPE_2025)
    * TIPE 2024 - Go-getter : résolution algorithmique et génération de nouveaux problèmes : [Page GitHub](https://github.com/Sacss-dev/TIPE_2024)

